---
title: "Reflecting on My API Project"
description: Reflection on one of my school projects, how it worked, and why its core concepts matter.
date: 2026-06-01
image: "../public/hero/Bee_on_flower.jpg"
author:
  name: Peter Delwita
---

One of the projects I worked on when I took AP Computer Science Principles involved pulling from an NYC Open Data API to put cards on the screen based on the API. I used Visual Studio Code to make the website, and the website was built using Vite + TailwindCSS. 

The key concepts I had to understand to design the site were array methods and async functions, and I learned why they matter in web development through this project. An async function is designed to return a Promise when called and allows asynchronous code to be easily read. An essential part of these functions is the await keyword, which pauses its execution until a promise is rejected or resolved.

The await keyword works well with try/catch, which serves as error handling for an asynchronous function. Try/catch can be used to create guard clauses, which terminate the function early if it does not meet a certain condition. 

Here is the async function the site uses:

```javascript
async function getData(URL, filterMode) {
  try {
    const response = await fetch(URL);
    console.log(response); // Have to log this to see status
    if (response.status != 200) {
      // Log response for status
      throw new Error(response); // Guard clause
    } else {
      const data = await response.json(); // Turns response into json file we can use
      console.log(data);
      // Make cards
      data
        .filter((school) => filterCards(school, filterMode))
        .forEach((school) => {
          const cardHTML = `
            <div class="h-120 w-[25%] border-solid border-2 bg-gray-900 border-blue-600 rounded-2xl flex m-8 p-12 flex-wrap justify-center items-center max-[1150px]:w-[40%] max-[600px]:w-[80%] max-[350px]:p-8">
              <div class="container w-full flex justify-center">
                <h2 class="text-[21px] min-[800px]:text-[24px] text-blue-600 text-center" id="school-name">School Name: ${school.school_name}</h2>
              </div>
              <ul class="sat-avg-scores">
                <li class="text-[16px] text-blue-600" id="math-scores">DBN: ${school.dbn}</li>
                <li class="text-[16px] text-blue-600" id="math-scores">Math: ${school.sat_math_avg_score}</li>
                <li class="text-[16px] text-blue-600" id="writing-scores">Writing: ${school.sat_writing_avg_score}</li>
                <li class="text-[16px] text-blue-600" id="reading-scores">Critical Reading: ${school.sat_critical_reading_avg_score}</li>
                <li class="text-[16px] text-blue-600" id="test-takers">Number of Takers: ${school.num_of_sat_test_takers}</li>
            </div>
          `;
          DOMSelectors.container.insertAdjacentHTML(
            // Make some cards
            "beforeend",
            cardHTML
          );
        }); // Supposed to insert the cards into the container.
    }
    // Log names, scores of each school.
  } catch (error) {
    alert("School not found");
  }
}

getData(URL, "");
```
To use trycatch, I need to fetch the URL and use to response to create data, and I need to log the response to check the status. If the status is not 200, it throws an error to prevent the function from running further (guard clause). I have to use await when declaring the response and the data, and if the status is 200, I use array methods to filter through schools in the API and put their respective cards on the screen.

To make the cards, I declare the variable cardHTML and use backticks to contain the card HTML, then create them using insertAdjacentHTML within forEach. The parameters for getData are the URL and a filter mode. I left the filter mode blank when I called it to show all cards when the user loads up the website.

The function for filtering cards is shown below:

```javascript
function filterCards(school, filterMode) {
  // Run only when we change filtermode
  if (
    school.sat_critical_reading_avg_score === "s" ||
    school.sat_writing_avg_score === "s" ||
    school.sat_math_avg_score === "s" ||
    !school
  ) {
    return false; // Guard clause, remove invalid scores (don't list schools saying "s")
  }

  const defaultReading = 400;
  const defaultWriting = 400;
  const defaultMath = 400;

  const minReading =
    parseInt(DOMSelectors.inputCriticalReading.value) || defaultReading;
  const minWriting =
    parseInt(DOMSelectors.inputWriting.value) || defaultWriting;
  const minMath = parseInt(DOMSelectors.inputMath.value) || defaultMath;

  // Reading Scores
  if (filterMode === "reading") {
    return school.sat_critical_reading_avg_score >= minReading;
    // Writing Scores
  } else if (filterMode === "writing") {
    return school.sat_writing_avg_score >= minWriting;
    // Math Scores
  } else if (filterMode === "math") {
    return school.sat_math_avg_score >= minMath;
    // All Three
  } else if (filterMode === "all") {
    return (
      school.sat_critical_reading_avg_score >= minReading &&
      school.sat_writing_avg_score >= minWriting &&
      school.sat_math_avg_score >= minMath
    );
    // Filter by Borough
  } else if (filterMode === "manhattan") {
    return school.dbn.includes("M");
  } else if (filterMode === "bronx") {
    return school.dbn.includes("X");
  } else if (filterMode === "brooklyn") {
    return school.dbn.includes("K");
  } else if (filterMode === "queens") {
    return school.dbn.includes("Q");
  } else if (filterMode === "statenisland") {
    return school.dbn.includes("R");
    // No Filter
  } else {
    return true;
  }
}
```
Each time one of the buttons is pressed, cards are filtered based on the new filter mode the button corresponds to. The use of array methods is central to all of this. Array methods allow you to manipulate arrays with ease; you can modify elements, add them, remove them, filter through them, and more. The array methods I used were .filter and .forEach. Here they are in the project:

```javascript
      data
        .filter((school) => filterCards(school, filterMode))
        .forEach((school) => {
          const cardHTML = `
            <div class="h-120 w-[25%] border-solid border-2 bg-gray-900 border-blue-600 rounded-2xl flex m-8 p-12 flex-wrap justify-center items-center max-[1150px]:w-[40%] max-[600px]:w-[80%] max-[350px]:p-8">
              <div class="container w-full flex justify-center">
                <h2 class="text-[21px] min-[800px]:text-[24px] text-blue-600 text-center" id="school-name">School Name: ${school.school_name}</h2>
              </div>
              <ul class="sat-avg-scores">
                <li class="text-[16px] text-blue-600" id="math-scores">DBN: ${school.dbn}</li>
                <li class="text-[16px] text-blue-600" id="math-scores">Math: ${school.sat_math_avg_score}</li>
                <li class="text-[16px] text-blue-600" id="writing-scores">Writing: ${school.sat_writing_avg_score}</li>
                <li class="text-[16px] text-blue-600" id="reading-scores">Critical Reading: ${school.sat_critical_reading_avg_score}</li>
                <li class="text-[16px] text-blue-600" id="test-takers">Number of Takers: ${school.num_of_sat_test_takers}</li>
            </div>
          `;
          DOMSelectors.container.insertAdjacentHTML(
            // Make some cards
            "beforeend",
            cardHTML
          );
        }); // Supposed to insert the cards into the container.
```
.filter and .forEach both rely on the use of arrow functions to be carried out in this context. The filterCards function is designed to exclude cards based on certain conditions, such as math scores and borough, and it is used in the .filter array method with its corresponding filter mode. After that, .forEach is used to make cards for the schools that passed the filter, and those are the cards that are shown to the user when they press buttons to filter the schools. This made it very convenient to work with the API; the hard part was coming up with the async function, but handling the API did not require much thanks to these array methods.

The importance of these concepts was what clicked for me as I worked on this project. APIs help make projects more scalable and efficient since they help save development time and because they allow for data to be easily communicated between systems. If you are programming something relying on backend development (such as a social media site), pulling from an API will be of utmost importance. Many websites that offer services do this as well. Building a huge data structure on your own is very difficult, but it's much easier to pull from an API to achieve your ends when designing your website.

It also matters how we work with APIs, and array methods can make this particularly easy in some areas; if we only want certain elements from the API based on certain conditions, we can use .filter to do that in few lines of code. If we want to iterate through an API, we can use .forEach. I learned how useful that was when I used it to manage the data from the API I was using, making it easy to accomplish the task of putting cards on the screen based on certain conditions, which would have been much more difficult without them. Likewise, it would have been difficult to create an API on my own just for this project, but it was much easier using one from NYC Open Data.

In short, APIs make it much easier to create scalable and efficient applications, and array methods make it easy to handle huge data sets, and are thus useful for working with APIs. Both of these matter when it comes to building large applications. That was what I learned when I did this project.