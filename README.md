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
# game name model
class GameName(models.Model):
    game_name = models.CharField(max_length=60, default="")

    objects = models.Manager()

    def __str__(self):
        return self.game_name

PLATFORM_CHOICES = (...)

# creating a speedrun class object
class Record(models.Model):
    player = models.CharField(max_length=60, default="")
    game = models.ForeignKey(GameName, related_name='records', on_delete=models.CASCADE)
    time = models.CharField(max_length=30, default="HH:MM:SS")
    platform = models.CharField(max_length=60, choices=PLATFORM_CHOICES, default="")
    date = models.DateField(default=date.today)

    objects = models.Manager()

    def __str__(self):
        return [self.player]
```


To create a new record users needed to click on the "Add Record" link in the navbar. The rendered page contained a list of form inputs including a dropdown menu containing games already entered in the database. If the user's game wasn't listed, there was a button that linked to another page with a one-input form where they could add their game and be redirected back to the now updated "Add Record" form. This meant users would store their entries with the correct game names. This was accomplished using the following functions:
```
# create speedrun record form using model form in django
def add_record(request):
    form = SpeedrunForm(data=request.POST or None)
    if form.is_valid():
        form.save()
        return redirect('speed_run_home')
    else:
        print(form.errors)
        form = SpeedrunForm()
        context = {'form': form}
    return render(request, 'speed_run_create.html', context)


# create game form using model form in django
def add_game(request):
    form = GameForm(data=request.POST or None)
    if form.is_valid():
        form.save()
        return redirect('speed_run_create')
    else:
        print(form.errors)
        form = GameForm()
        context = {'form': form}
    return render(request, 'speed_run_create_game.html', context)
```

## <a name="story3"></a>Story #3: Database Item Display
I wanted the database items displayed to be contingent upon the game the user was interested in, so I made a page that just listed all games currently in the database and select one to see all records associated with it. This was the function I used: 
```
# displays a list of all games entered as GameName Objects
def all_games(request):
    games = GameName.objects.all()
    return render(request, 'speed_run_all_games.html', {'games': games})
```

This brought up a page with a list of all speedruns in the database for that particular game. To do this I had to call all entries from the Record object associated with the selected entry from the GameName object through the "game" foreign key. The following is the function that I finally landed on: 
```
# passes Record information if player has a record for the selected game
def game_record(request, pk):
    gamename = get_object_or_404(GameName, pk=pk)
    records = Record.objects.filter(game=gamename).order_by("time")
    content = {'gamename': gamename, 'records': records}
    return render(request, 'speed_run_game_records.html', content)
```

## <a name="story4"></a>Story #4: Details Page
The following function displayed a dedicated details page for any entry clicked on in the game record list on the previous page. 
```
# displays all details relevant to a selected player
def speed_run_details(request, pk):
    details = get_object_or_404(Record, pk=pk)
    content = {'details': details}
    return render(request, 'speed_run_details.html', content)
```

## <a name="story5"></a>Story #5: Item Edit and Delete
Edit and Delete buttons were added to the details template page, each leading to their own page. This function displays the information for the selected record in a form that allows the user to make changes and overwrite the existing entry. 
```
# allows the user to edit an entry
def speed_run_edit(request, pk):
    item = get_object_or_404(Record, pk=pk)
    form = SpeedrunForm(data=request.POST or None, instance=item)
    if request.method == 'POST':
        if form.is_valid():
            form2 = form.save(commit=False)
            form2.save()
            return redirect('speed_run_all_games')
        else:
            print(form.errors)
    else:
        return render(request, 'speed_run_edit.html', {'form': form, 'item': item})
```

This function allows users to remove a speedrun record from the database. The Delete button on the record details page links to a separate template that displays a final confirmation warning asking if they are sure they want to permanently delete the entry. 
```
# allows users to delete a record
def speed_run_delete(request, pk):
    item = get_object_or_404(Record, pk=pk)
    if request.method == 'POST':
        item.delete()
        return redirect('speed_run_all_games')
    context = {'item': item}
    return render(request, 'speed_run_delete.html', context)
```

## <a name="story6"></a>Story #6/7: API and JSON Parse
The web application also utilized an API to retrieve information about video games. Via the API template page users could enter a term in the search bar and details about games would be displayed in card format including pictures, titles, ratings, and dates of release. The following function took the input, got a JSON response, and parsed that response, returning only the desired information for each related title. 
```
def speed_run_api(request):

    search_list = []

    if request.method == 'POST':

        url = "https://rawg-video-games-database.p.rapidapi.com/games?key=5f37df7ee53e491f847a43dfa97cb3a2"

        headers = {
            'x-rapidapi-host': "rawg-video-games-database.p.rapidapi.com",
            'x-rapidapi-key': "9afece8438msh5f25fff510a60bbp1954d2jsn7f98f53b6d37"
        }

        params = {"search": request.POST['searchItem']}

        response = requests.request("GET", url, headers=headers, params=params)

        game_data = json.loads(response.text)

        for game in game_data['results']:
            bg_img = game['background_image']
            title = game['name']
            release = game['released']
            rating = game['rating']
            game_array = (bg_img, title, release, rating)
            search_list.append(game_array)

    content = {
        'search_list': search_list
    }

    return render(request, 'speed_run_api.html', content)
```


## <a name="conclusion"></a>Conclusion
1.	Through this project I became familiar with Python, Django, Object Oriented Programming, and database manipulation.
2.	I gained experience in collaborative team-based work and with Agile/Scrum methodologies, DevOps, and Azure DevOps services.
3.	As the Live Project relied on version control, I practiced making pull requests, merging, committing and pushing changes, and creating branches. 
4.	I encountered numerous challenges on this project and learned how to process them, not panic, and conduct efficient constructive research.
