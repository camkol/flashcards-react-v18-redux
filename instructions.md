# Challenge Project: Flashcards

### Overview

Instead of a step-by-step tutorial, this project contains a series of open-ended requirements that describe the project you’ll be building. There are many possible ways to correctly fulfill all of these requirements, and you should expect to use the internet, Codecademy, and other resources when you encounter a problem that you cannot easily solve.

### Project Goals

In this project, you will practice using Redux and Redux Toolkit to manage the complex state of a flashcard-style quiz app. Users will be able to create their own topics, quizzes for those topics, and flashcards for those quizzes. Users will also be able to interact with their quizzes by flipping flashcards over.
The following task descriptions will walk through implementing the app’s Redux logic, starting with topics, then quizzes, and then cards. If you would like to implement it in a different order, feel free to do what is comfortable for you.

### Setup Instructions

If you choose to do this project on your computer instead of Codecademy, you can download what you’ll need by clicking the “Download” button below. Make sure you have Node installed on your computer and then, inside the project’s root directory, run `npm install`. Finally, run `npm start` which will automatically open up a new tab in your browser with your running application. If a new tab does not appear, you can visit http://localhost:3000/.

## Tasks

0/18 complete

### Prerequisites

1. This app uses `uuidv4()` function from the `uuid` package to create unique identifiers for topics/quizzes/cards. Below, you can see an example of how this function is used:

```javascript
import { v4 as uuidv4 } from "uuid";

let uniqueId = uuidv4();

console.log(uniqueId); // Prints '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
```

Though not required, if you would like to learn more about this function, check out its [documentation](https://www.npmjs.com/package/uuid#uuidv4options-buffer-offset).

2. This app uses `react-router` to handle routing between different pages. Since `react-router` is outside the scope of this project, we’ve written the routing code for you. Though not required, if you’re curious about how it works, you can explore App.js (where the routes for this app are defined), read the [`react-router` docs](https://reactrouter.com/en/main), or take our [Learn React Router](https://www.codecademy.com/enrolled/courses/learn-react-router) course!

### Project Requirements

3. At a high level, your application will be able to handle the following URL routes, each with their own functionality:

   On the `'/topics/new'` page:

   - Users can create topics

   On the `'/topics'` page: \* Users can view all topics \* Users can click on an individual topic and be redirected to the page for that topic

   On the `/topics/:topicId` page:

   - Users can view an individual topic and all quizzes for that topic \* Users can click on a quiz associated with a topic and be redirected to that quiz’s page

   On the `'quizzes/new'` page:

   - Users can create quizzes that are associated with topics and contain lists of flashcards \* Users can add and remove card fields in the new quiz form

   On the '/quizzes' page:

   - Users can view all quizzes \* Users can click on an individual quiz and be redirected to that quiz’s page

   On the `'/quizzes/:quizId'` page:

   -Users can view an individual quiz and flip cards over

4. Before you start writing code, take a moment to review our recommended state structure:

- Your app will include three slices: one for topics, one for quizzes, and one for cards.
- Each slice’s state should include an object storing all the topics/quizzes/cards keyed by their `id`. This will allow you to quickly retrieve an object’s information whenever you need to look it up.
- Each individual quiz will have a `topicId` value corresponding to an individual topic in state.
- Similarly, each topic which will have a `quizIds` array corresponding to the associated quizzes in state.
  All together, your app state will look like this:

```javascript
{
  topics: {
    topics: {
      '123': {
        id: '123',
        name: 'example topic',
        icon: 'icon url',
        quizIds: ['456']
      }
    }
  },
  quizzes: {
    quizzes: {
      '456': {
        id: '456',
        topicId: '123',
        name: 'quiz for example topic',
        cardIds: ['789', '101', '102']
      }
    }
  },
  cards: {
    cards: {
      '789': {
        id: '789',
        front: 'front text',
        back: 'back text'
      },
      '101': {
        id: '101',
        front: 'front text',
        back: 'back text'
      },
      '102': {
        id: '102',
        front: 'front text',
        back: 'back text'
      },
    }
  }
}
```

**Hint**

You might be wondering why, in each slice, we are asking you to include a property with the same name as the slice itself (for example `state.topics.topics`). This app is relatively small, but you can imagine that in a more complex application you might want to store multiple groupings of topics, quizzes, or cards in one slice in state.

For example, if you allowed users to `favorite topics`, you might store a favoriteTopics in your topics slice. By storing all of the topics under a named key, rather than deciding that they should comprise the entire slice, you are creating a flexible state structure that can grow as you add functionality to your app.

This is how we recommend you structure your state, but you are free to adjust it if you wish. Just know that the rest of the instructions assume you are using this structure.

5. Your first task is to write code to manage the state associated with topics. In the **src/features/topics** directory, create a new file containing a slice that:

   - Is named `topicsSlice`.
   - Has initial state consisting of an object that includes one property, `topics`, which corresponds to an empty object. This inner `topics` object will eventually hold all topics keyed by id.
   - Has an `addTopic` action. You can expect the payload for this action to look like `{id: '123456', name: 'name of topic', icon: 'icon url'}`. Store these values in the state as a new topic object.
   - Each topic object added to the state should also have a `quizIds` property, which will correspond to an array containing the `id`s of each quiz associated with the topic. When a topic is first created, it won’t have any associated quizzes, but you should still create an empty `quizIds` array so that all topics in the state conform to the same shape.
   - Create a selector that selects the topics object nested within `initialState`.
   - Export the selector as well as the action creators and reducer that your slice generates.

**Hint**

- Use `createSlice` to generate your topics slice. Your `addTopic` action should modify the state.topics object by adding an object representing a single topic to the `state.topics` object. Remember, we want the `state.topics` object to be keyed by the topic’s `id` which you receive in the action’s `payload`.
- You can use [computed property](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#computed_property_names) names to add values to an object using a variable as a key:

```javascript
const obj = {
  [key]: value,
};
```

6.  Add the `topics` slice to the app’s store.

**Hint**

You’ll need to import the reducer generated by your topics slice in **src/app/store.js** and add it to the empty object passed to `configureStore` under the key `topics`.

7.  In **src/features/topics/Topics.js**, import the selector defined in your slice and use it to access all the `topics` in state, and replace the empty object currently assigned to topics with the topics in state.

**Hint**

You will need to import `useSelector` and then call it in conjunction with your selector to access all the topics in state.

8.  Next, you’ll need to hook the new topic form up to the action creators your slice generates. In **src/components/NewTopicForm.js**, import `addTopic` and dispatch it from the event handler that runs when the new topic form is submitted.

Verify that your code is working by filling out the form and submitting it. You should be redirected to the `/topics` page and should see your newly created topic there.

**Hint**

- You will need to import and call the `useDispatch()` method in order to dispatch actions to the store.
- You will need to include the topic’s `name` in the action payload as well as an `id` property (you should generate a value for this property by calling `uuidv4()`) and an `icon` property.
- The `icon` variable from the `useSelect()` hook holds the URL for the icon image selected by the user.

9.  Great work! Now that you can create topics, your next task is to build out the necessary functionality to add quizzes to your app. This will involve creating two new slices—one for the quizzes themselves and one for the cards that comprise them—and adding an action to your topics slice to associate quizzes with the topic to which they belong. To start, create in the **src/features/quizzes** directory, create a new file containing a slice for quizzes that:

- Is named `'quizzesSlice'`
- Has initial state consisting of an object that includes one property, `quizzes`, which corresponds to an empty object. This inner `quizzes` object will eventually hold all quizzes keyed by `id`.
- Has an `addQuiz` action. This action will receive a payload of the form `{ id: '123', name: 'quiz name', topicId: '456', cardIds: ['1', '2', '3', ...]}`.
- Has a selector which returns all quizzes from state.
- Export the selector as well as the action creators and reducer that your slice generates.
- Is added to the store.

**Hint**

- Use `createSlice` to generate your quizzes slice.
- Your `addQuiz` action should modify the `quizzes` object in state by adding an object representing a single quiz to the `quizzes` object.
- Remember, we want the `quizzes` object to be keyed by quiz `id`, so insert your newly created quiz object as the value associated with the id you receive in the action’s `payload`.

10. Next, you should add an action to your topics slice that adds a quiz’s id to the quizIds array of the topic with which the newly created quiz is associated. This action will receive the same payload the quizzes slice addQuiz action received in the form { id: '123', name: 'quiz name', topicId: '456', cardIds: ['1', '2', '3', ...]}.
    **Hint**
    Use the payload’s topicId to find the correct topic in state, and push the payload’s id into that topic’s quizIds array.
    You’ll want to make use of the topicsSlice‘s extraReducers to respond to a quizzesSlice "addQuiz" action.
11. To test your work, you’ll need to connect your action creator to src/components/NewQuizForm and make sure the component works. First, import your topics selector from your topics slice and replace the variable topics, which is currently assigned an empty object, with a call to that selector.
    **Hint**
    To use your selector you will need to call useSelector with the selector you defined in your topics slice.
12. Next, import the action from your quiz slice and dispatch it from the handleSubmit() event handler that fires when the new quiz form is submitted.

- Remember, that action creator expects to receive a payload of the form { id: '123', name: 'quiz name', topicId: '456', cardIds: ['1', '2', '3', ...]}. You’ll have to generate an id by calling uuidv4. For now, pass the empty cardIds array variable for the cardIds property (you’ll change that in a later task).
- Test that your action creator works by filling out the new quiz form. After your quiz is created you should be rerouted to the /quizzes page and should see your newly created quiz there.
  Note: If you are doing this project on Codecademy.com, the URL bar may not change (but the user interface should!)
  **Hint**
  Your dispatch statement should look something like:
  dispatch(
  addQuiz({
  name: '...',
  topicId: '...',
  cardIds: [],
  id: '...',
  })
  );

13. Lastly, import your selector in src/features/quizzes/Quizzes.js, src/features/quizzes/Quiz.js, and src/features/topics/Topic.js and make sure those components are displaying the correct data:

- The Quizzes component should render a Link for each quiz value in the quizzes slice of state.
- The Quiz component uses the react-router-dom method useParams() to determine the quizId to render. Therefore, it needs the full set of quizzes to find the appropriate quiz object to render.
- The Topic component should replace the empty object assigned to quizzes with the selector.
  **Hint**
  Use useSelector in conjunction with your selectors to pull the all the quizzes from state.

14. Great work! Next, in the src/features/cards directory, create a new file containing slice for cards that:

- Is named 'cardsSlice'
- Has initial state consisting of an object that includes one property, cards, which corresponds to an empty object. This inner cards object will eventually hold all cards keyed by id.
- Has an addCard action. This action will receive a payload of the form { id: '123', front: 'front of card', back: 'back of card'}.
- Has a selector that returns a card with the given id.
- Is added to the store.
  **Hint**
  Use createSlice to generate your cards slice. addCard should insert the payload into the cards object in state as the value associated with the id in the payload.
  You can use the following pattern to pass arguments to a selector:
  const mySelectorByName = (name) => (state) => state.items.find(item.name === name));

15. Lastly, connect your addCard action creator to the new quiz form. In src/components/NewQuizForm, in the event handler that fires when the quiz form is submitted, iterate through the cards in that form’s local state, and for each one:
16. dispatch your addCard action creator. You will have to generate an id for each card using uuidv4.
17. Store the id you create for each card in the cardIds array we’ve provided for you. Remember, your action creator expects to receive a payload of the form { id: '123', front: 'front of card', back: 'back of card'}. You want to collect all the cardIds in an array so that you can pass them to the action creator that generates new quizzes. To use uuidv4 to create an id, call the function like so: uuidv4().
    **Hint**
    You can loop through each of the cards like so:
    const cardIds = [];

cards.forEach((card) => {
let cardId = uuidv4();
cardIds.push(cardId);
dispatch(addCard({ ...card, id: cardId }));
});

16. You previously passed an empty array for cardIds to the action creator that generates a new quiz. Now that you have written code to collect an array of all the cardIds created whenever the new quiz form is submitted, replace the empty array with this array of cardIds.
    To test that your code is working, create a new quiz with some cards. Navigate to that quiz from the /quizzes page, and verify that your cards show up. Flip them over by clicking on them to make sure that you’ve correctly captured all of the state belonging to each card.
    Hint
    Since you’re already dispatching the action that generates a new quiz with an appropriately structured payload, all you need to do is replace the empty array you previously assigned to cardIds with an array that contains the ids belonging to the cards associated with the quiz that’s being created.
17. Now that you can add new cards, you’ll need to display cards on the individual quiz page. The Quiz component renders a list of Card components, so in src/features/cards/Card.js, import your cards selector and use it to access all the cards in state.
    Hint
    Use useSelector in conjunction with your selector to get a card by id.
    You can use the following pattern:
    const item = useSelector(mySelectorByName("test1"));

Solution 18.
Great work! Visit our forums to compare your project to our sample solution code. You can also learn how to host your own solution on GitHub so you can share it with other learners! Your solution might look different from ours, and that’s okay! There are multiple ways to solve these projects, and you’ll learn more by seeing others’ code.
