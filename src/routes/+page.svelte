<script>
    // Imports from firebase
    import { initializeApp, getApps, getApp } from "firebase/app";
    import { getFirestore, collection, onSnapshot, doc, updateDoc, deleteDoc, addDoc } from "firebase/firestore";
    // Import the information for my specific Firebase instance
    import { firebaseConfig } from "$lib/firebaseConfig";
    // browser is "true" if the web app is running in the browser
    import { browser } from "$app/environment";

    // Before defining firebaseApp, check that the app is running in the browser
    // getApps returns a list of all initialized Firebase apps
    // If there are no initialized apps, we initizalize an app by passing firebaseConfig (defined in lib folder)
    // into the initializeApp() function
    // Otherwise, getApp() returns the default initialized Firebase app
    let firebaseApp = browser && (getApps().length === 0 ? initializeApp(firebaseConfig) : getApp());
    
    // Check that the app is running in the browser and if so, initialize Cloud Firestore and get a reference to the service
    const db = browser && getFirestore(firebaseApp);
    // Get a reference to the specific collection called "todos", which I manually created in the Firestore dashboard
    const colRef = browser && collection(db, "todos");

    // Initialize empty list of todo item to start with
    let todos = [];

    // This method lets us get a snapshot of the state of the database at the start of the web app
    const unsubscribe =
        // Check that the app is running in the browser
        browser &&
        // This checks the collection "todos" in our database
        onSnapshot(colRef, (querySnapshot) => {
            // Initialize a variable for the todos stored in the Firebase database
            let fbTodos = [];
            // For each todo item stored in Firebase, add it to fbTodos
            querySnapshot.forEach((doc) => {
                let todo = {...doc.data(), id: doc.id};
                fbTodos = [todo, ...fbTodos];
            });
            // Set the local todos variable equal to fbTodos so that we can displayed todo items from a previous session
            // This way, the web app's list of todo items does not reset every time the user refreshes the page
            todos = fbTodos;
        });

    // Initialize task and error variables
    let task = "";
    let error = "";

    // This function allows the app to add a new todo item to the list
    // "async" function because we need to wait for addDoc to input the new todo item in Firebase
    const addTodo = async () => {
        // Make sure that the user has actually input a task in the input textbox
        if (task !== "") {
            // call addDoc function and pass a todo item into the "todos" collection in our Firebase db
            const docRef = await addDoc(collection(db, "todos"), {
                task: task,
                isComplete: false,
                createdAt: new Date(),
            });
            error = "";
        } else {
            // If the user clicked enter or pressed the Add button without text in the input box,
            // show an error to the user
            error = "Task is empty";
        }
        // Reset the input text box
        task = "";
    };

    // Update the completion status of a todo item in the database
    const markTodoAsComplete = async (item) => {
        // The updateDoc function will update the element in the "todos" collection with item id "item.id"
        // such that its associated field "isComplete" is set to the opposite of its current value
        await updateDoc(doc(db, "todos", item.id), {
            isComplete: !item.isComplete,
        });
    };

    // Delete a todo item from the database
    const deleteTodo = async (id) => {
        // Call the deleteDoc function which deletes an item from the "todos" collection with item id "id"
        await deleteDoc(doc(db, "todos", id));
    };

    // Allow users to add a todo item with the Enter key
    const keyIsPressed = (event) => {
        // When a user clicks Enter, trigger the addTodo() function
        if (event.key == "Enter") {
            addTodo();
        }
    }
</script>

// Create an input field where the user can input their todo tasks
// The input that the user enters will be associated with the variable "task"
<input type="text" placeholder="Add a task" bind:value={task} />

// Create an Add button, which triggers addTodo() when clicked
<button on:click={addTodo}>Add</button>

// Create a list of the todo items
<ol>
    // For each todo item
    {#each todos as item}
        // Create a list-item whose class is "complete" if the item's isComplete field is true
        // The "complete" class will make the list item appear to be crossed out
        <li class:complete={item.isComplete}>
            // Display the actual task
            <span>
                {item.task}
            </span>
            <span>
                // Display a check-mark button that when clicked will call the markTodoAsComplete()
                // which will set the isComplete variable to the opposite of its current value
                <button on:click={() => markTodoAsComplete(item)}>✓</button>
                // Display an X-mark button that deletes the todo item by calling deleteTodo()
                <button on:click={() => deleteTodo(item.id)}>✗</button>
            </span>
        </li>
    {:else}
        // If there are no todo items to iterate through, display this message instead
        <p>No todos found</p>
    {/each}
    // Display the error message, which is blank ("") if the user has not tried to add a blank item 
    // to the todo list
    <p class="error">{error}</p>
</ol>

// Add an event (keydown) listener for the window object
// Handle the "keydown" event by calling keyIsPressed() as defined above
<svelte:window on:keydown={keyIsPressed} />

// Styling for the todo list
<style>
    .complete {
        text-decoration: line-through;
    }
    .error {
        color: red;
    }
</style>