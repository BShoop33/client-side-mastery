# Releasing Happy Animals From Your Care

In this chapter, you are going to provide the ability to release animals from your care. Once an animal is ready to go home with their owner, it doesn't need to be in your system any more, so you need to delete it from your database of active animals.

## Add a Release Button

First, add a button to your animal details component that will allow the user to release the animal from care.

> ##### `/src/components/animal/AnimalDetails.js`

```jsx
<button className="btn--release"
        onClick={() => {
            // Code to delete animal from database
        }}
>Release</button>
```

## Implement and Expose DELETE Method in Provider

Since the provider components handle all interactions with the database, you need to implement a function that performs a `fetch` operation with the **DELETE** method to delete a specific animal.

> ##### `/src/components/animal/AnimalProvider.js`

```js
const releaseAnimal = animalId => {
    return fetch(`http://localhost:8088/animals/${animalId}`, {
        method: "DELETE"
    })
        .then(getAnimals)
}
```

Expose the method via the _AnimalContext_.

> ##### `/src/components/animal/AnimalProvider.js`

```jsx
<AnimalContext.Provider value={{
    animals, addAnimal, releaseAnimal
}}>
```


## Use Provider Method

Get a reference to the release function in your animal component.

> ##### `/src/components/animal/AnimalDetails.js`

```js
// Update this line of code to include releaseAnimal
const { animals, releaseAnimal } = useContext(AnimalContext)
```

And then invoke the function when the button is clicked. Once the delete operation is complete, redirect the user back to the list of animals.

> ##### `/src/components/animal/AnimalDetails.js`

```jsx
<button className="btn--release"
        onClick={() => {
            releaseAnimal(chosenAnimalId)
                .then(() => {
                    props.history.push("/animals")
                })
        }}
>Release</button>
```