# Python-Django Live Project Code Summary 

## <a name="intro"></a>Introduction
Recently, I took part in a 2-week sprint for the Python Live Project as part of my studies at The Tech Academy's Software Developer Bootcamp. During this time I created a web application using the Python language and Django Framework. The goal of the 2-week sprint was to create a functional web app capable of creating, editing, viewing, and deleting information in a database. Working on this project deepened my understanding of not just Python and Django, but also computer logic. I learned how to link the different files necessary in a Django-based site, use functions to retrieve information from the database, manipulate that information, and display it on a dedicated HTML template. 

Working in an environment that utilized the Agile/Scrum methodology and DevOps was a valuable experience that taught me the importance of breaking projects and challenges down into smaller, more manageable tasks as well as having a structure in place to organize workflow. Communication proved to be a vital practice in this environment, even outside of daily standups, sprint planning, and sprint retrospectives. Asking for help and sharing problems and challenges in addition to successes was beneficial to not only myself, but the entire team and the project as a whole. 

Stories were assigned through Azure DevOps and git version control was used to maintain project integrity. Using PyCharm and Azure, I learned how to create working branches, merge the master to the working branch, commit and push changes to the remote branch, and resolve any resulting merge conflicts. Following story resolution, I would use Azure DevOps to create a pull request then move my story to the corresponding column in the Boards section. 

## Table of Contents
* [Story 1: Build the Basic App](#story1)
* [Story 2: Model Creation](#story2)
* [Story 3: Database Item Display](#story3)
* [Story 4: Details Page](#story4)
* [Story 5: Item Edit and Delete](#story5)
* [Story 6/7: API and JSON Parse](#story6)
* [Conclusion](#conclusion)

## <a name="story1"></a>Story #1: Build the Basic App
For this Live Project I came up with the idea to create a web application that stored and displayed video game speedrunning statistics. In the first story I had to create a new app and link it to the necessary files within the main project. Then, I needed to create the basic structure of my Django app including templates and a function to render the home page. 

## <a name="story2"></a>Story #2: Model Creation
Creating a model that allows users to enter their speedrunning records posed certain challenges. I wanted to be able to display all entries from a particular game title in a list. Normally, I would have created a separate model for individual games, because each game is so different and so that entries would be safely stored in separate tables with predefined names. However, it was impractical to create individual game models, and I couldn't create a choices list with every game contained in it, so I made two tables--one solely for users to input games and another that contained a generic list of speedrun attributes with a foreign key link to game names that displayed as a choices list. 

Here are my models:
```

```


To create a new record users needed to click on the "Add Record" link in the navbar. The rendered page contained a list of form inputs including a dropdown menu containing games already entered in the database. If the user's game wasn't listed, there was a button that linked to another page with a one-input form where they could add their game and be redirected back to the now updated "Add Record" form. This meant users would store their entries with the correct game names. 

## <a name="story3"></a>Story #3: Database Item Display



## <a name="story4"></a>Story #4: BDetails Page



## <a name="story5"></a>Story #5: Item Edit and Delete



## <a name="story6"></a>Story #6/7: API and JSON Parse



## <a name="conclusion"></a>Conclusion
1.	Through this project I became familiar with Python, Django, Object Oriented Programming, and database manipulation.
2.	I gained experience in collaborative team-based work and with Agile/Scrum methodologies, DevOps, and Azure DevOps services.
3.	As the Live Project relied on version control, I practiced making pull requests, merging, committing and pushing changes, and creating branches. 
4.	I encountered numerous challenges on this project and learned how to process them, not panic, and do efficient constructive research.
