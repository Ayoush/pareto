So `promises` as the name suggest is contract or agreement to continue with your work and once an action happen based on the result the given `promised` operation will either get `resolved` or `rejected`. This is referred to as a `asynchronous` operation. 

Promises are well explained with example. Lets take most common used example of promises that is *Fetching some data and processing it*. In our example we will fetch userdata and process it.

- Fetch users data from the API
- Process the data
- Handle the errors
- Ensure cleanup operations

```js
function fetchUser(userID) {
	return new Promise((resolve, reject) => {
		console.log("fetching user data");
		setTimeout(() => {
			if(userID == 1){
				resolve({id: 1, name: "John Dee", dob: "1990-05-21"});
			}else{
				reject("User not found");
			}
		}, 5000);
	});
}

function calculateAge(dob) {
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			const birthYear = new Date(dob).getFullYear();
			const currentYear = new Date().getFullYear();
			const age = currentYear - birthYear;
			if (isNaN(age)) {
			    reject("Invalid date of birth!");
			} else {
				resolve(age);
			}
		}, 4000);
	});
}

fetchUserData(1).then((user) => {
		console.log("User fetched:", user);
		return calculateAge(user.dob);
	}).then((age) => {
		console.log(`User's age is ${age}`);
	}).catch((error) => {
		console.error("Error occurred:", error);
	}).finally(() => {
		console.log("Operation complete. Cleaning up resources...");
});

```

Some key points :-
- `Chaining Promises`:-
	- each `then` receives the resolve of the previous promise.
	- Helps breaking down complex operations
- `catch` error handling:-
	- catch handles any rejections in the chain, whether it occurs in `fetchUserData` or `calculateAge`
	- Prevents unhandled rejects so that your app doesn't crash
- `finally`
	- it runs, irrespective of whether the `Promise` chain resolves or rejects
	- Ideal for cleanup tasks like closing a database connection, loggin or resetting a loading spinner.
	- *It doesn't get value from resolve or reject*, so use it only for side effects and not for data processing

