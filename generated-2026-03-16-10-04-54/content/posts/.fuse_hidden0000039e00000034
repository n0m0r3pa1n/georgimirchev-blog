---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:20"
_publicize_job_id: "43819519291"
author: gmirchev90
categories:
  - android
  - code-architecture
date: "2020-05-04T10:49:53+00:00"
guid: http://georgimirchev.com/?p=109
parent_post_id: null
post_id: "109"
summary: |-
  ## Short Intro

  For 4 months now I have been implementing “clean architecture” in my current project. The project is huge, more than 17 Android developers work on it including me, so it is important to apply an architecture that works for all of us. I will not talk about what “clean architecture” is, but I will directly dive into the code and explain step by step the purpose of the layers that we have and what each layer is about.
tags:
  - clean-architecture
  - mvp
timeline_notification: "1588589395"
title: Applying Clean Architecture in Android
url: /2020/05/04/applying-clean-architecture-in-android/

---
## Short Intro

For 4 months now I have been implementing “clean architecture” in my current project. The project is huge, more than 17 Android developers work on it including me, so it is important to apply an architecture that works for all of us. I will not talk about what “clean architecture” is, but I will directly dive into the code and explain step by step the purpose of the layers that we have and what each layer is about.

## Where to  learn about “clean architecture”

There are many articles about it, so I will try to enumerate some of them here:

- Android 10 [Github Example](https://github.com/android10/Android-CleanArchitecture)
- Architecting Android the Clean Way [Article](https://fernandocejas.com/2014/09/03/architecting-android-the-clean-way/)
- [Google Samples](https://github.com/googlesamples/android-architecture/tree/todo-mvp-clean/) of Clean Architecture
- Five Agency [article](http://five.agency/android-architecture-part-3-applying-clean-architecture-android/) on Clean Architecture
- Various detailed guides on [Medium](https://medium.com/@dmilicic/a-detailed-guide-on-developing-android-apps-using-the-clean-architecture-pattern-d38d71e94029)

Much more information is available on Google, you just need to [search for it](http://bfy.tw/EzI0).

You will also need to know what is MVP and how it is used in Android. This will be our presentation layer only that will talk with the other layers. There is again [a ton of information](http://bfy.tw/EuMw) about MVP for Android:

- Medium [Article](https://medium.com/@cervonefrancesco/model-view-presenter-android-guidelines-94970b430ddf)
- Antonio Leiva [article](https://antonioleiva.com/mvp-android/)
- Google [Samples with MVP](https://github.com/googlesamples/android-architecture/tree/todo-mvp/)

## Let’s start

With everything clear let’s start with the simple example that I will be working on while introducing clean. We will build an app to display a collection of movies from an API. It will have a list of movies and a simple details screen for each movie. Let’s not focus on the design as it is not my primary goal.

### Libraries the project will use

- [Dagger](https://web.archive.org/web/20190723001008/https://google.github.io/dagger/) for dependency injection
- [RxJava](https://web.archive.org/web/20190723001008/https://github.com/ReactiveX/RxJava) + [Retrofit](https://web.archive.org/web/20190723001008/http://square.github.io/retrofit/) for API calls
- Test tools like [JUnit](https://web.archive.org/web/20190723001008/http://junit.org/junit5/) & [Mockito](https://web.archive.org/web/20190723001008/http://site.mockito.org/)
- [Annimon](https://web.archive.org/web/20190723001008/https://github.com/aNNiMON/Lightweight-Stream-API) [stream](https://web.archive.org/web/20190723001008/https://github.com/aNNiMON/Lightweight-Stream-API) because I am lazy when filtering lists

## Presentation Layer – MVP

Let’s start with MVP as our presentation layer.

### Presenter

Our presenter is the class with which the View communicates. We aim to have a very passive(“dumb”) view that does not contain any business logic.

#### startPresenting & stopPresenting

We have agreed that each of our presenters will have methods called `startPresenting` and `stopPresenting`. These methods are required because of the Disposables that need to be managed from RxJava. In `startPresenting` we save the reference to our View and in `stopPresenting` we clear all Disposables meaning that we stop all network requests from executing.

#### View Interface

Each Presenter exposes an interface which the view must implement in order to communicate with the presenter. The View interface is part of the Presenter class.

#### UiMappers

Each presenter may or may not have a UiMapper class. This is a class that receives a given type of data from the Interactor and then transforms it into a simpler data class that is used only for UI purposes. The main idea is that if you receive a class that has more responsibilities than the actual UI needs like it has unnecessary data or calls other classes, then you transform it to a simpler UI class with only display data.

#### Example

```
public class MoviesPresenter {

    private final GetMoviesInteractor getMoviesInteractor;
    private final MovieUiMapper movieUiMapper;
    private View view;
    private CompositeDisposable compositeDisposable;

    @Inject
    public MoviesPresenter(GetMoviesInteractor getMoviesInteractor, MovieUiMapper movieUiMapper) {
        this.getMoviesInteractor = getMoviesInteractor;
        this.movieUiMapper = movieUiMapper;
        compositeDisposable = new CompositeDisposable();
    }

    public void startPresenting(View view) {
        this.view = view;
    }

    public void stopPresenting() {
        compositeDisposable.clear();
    }

    public void searchForMovies(String search) {
        // Receive a List<Movie> -> List<UiMovie> to escape getActors method
        Disposable disposable = getMoviesInteractor.getMovies(search)
                .flattenAsObservable(movies -> movies)
                .map(movieUiMapper::toUiMovie)
                .toList()
                .subscribe(view::showMovies);
        compositeDisposable.add(disposable);
    }

    interface View {
        void showMovies(List<UiMovie> movies);
    }
}
```

### View

The view is our Activity. It implements from the interface our Presenter class exposes. It receives the Presenter from Dagger and whenever the user clicks search, it calls “Presenter.searchMovies.”. `startPresenting` and `stopPresenting` are called in `onResume` and `onPause`. It also implements the `showMovies` method which comes from the View interface. There it displays the movies when they are received bh the presenter.

#### Example:

```
public class MainActivity extends AppCompatActivity implements MoviesPresenter.View, MoviesAdapter.MovieClickListener {

    @Inject
    MoviesPresenter moviesPresenter;

    @Inject
    MovieNavigator movieNavigator;

    private RecyclerView moviesRecyclerView;
    private MoviesAdapter adapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        InjectHelper.getActivityComponent(this).inject(this);
        ***
    }

    private void searchForText(View view) {
        String search = ((EditText) findViewById(R.id.et_search)).getText().toString();
        moviesPresenter.searchForMovies(search);
    }

    @Override
    protected void onResume() {
        super.onResume();
        moviesPresenter.startPresenting(this);
    }

    @Override
    protected void onPause() {
        super.onPause();
        moviesPresenter.stopPresenting();
    }

    @Override
    public void showMovies(List<UiMovie> movies) {
        adapter.setMovies(movies);
    }

    @Override
    public void onMovieClick(UiMovie movie) {
        movieNavigator.openMovieDetails(movie.getId());
    }
}
```

### Navigator

This is a simple class we use when it comes to navigation. It is used to abstract the navigation code away as it is called directly from the classes that need to be responsible for the navigation between screens. It could contain also Fragment navigation. This Navigator can be used both in the Presenter layer or in the View layer.

```
public class MovieNavigator {
    private final Activity activity;

    public MovieNavigator(Activity activity) {
        this.activity = activity;
    }

    public void openMovieDetails(String id) {
        DetailsActivity.open(id, activity);
    }
}
```

## Interactors – App Business Rules Layer

### One thing at a time

As mentioned in all Clean guidelines, Interactors are representation of the Command pattern. Interactors are also called Use-Cases. Interactors are simple classes which contain only one public method and no more. They do only one thing. In our case we have **GetMoviesInteractor** and **GetActorsInteractor**. Both of them have only one method, either: getMovies or getActors.

### Business logic containers

Interactors make calls to Repositories to get a certain item in most of the cases. They contain all of the business logic that is needed to achieve the one thing they have to. That means if you have any filtering, sorting or whatever business logic that you need to implement, it is part of the Interactor.

### RxJava schedulers setup

Here is also the place where you set your Schedulers, not the Presentation layer.

### Interactors using Interactors or Repositories

It is a bad practice to have Interactors call other Interactors. An interactor can work with multiple repositories at once, but depending on other Interactors is bad as some of the business logic within interactors may change.

### Example

```
public class GetActorsInteractor {
    private final MoviesRepository moviesRepository;
    private final InteractorSchedulers schedulers;

    @Inject
    public GetActorsInteractor(MoviesRepository moviesRepository, InteractorSchedulers interactorSchedulers) {
        this.moviesRepository = moviesRepository;
        this.schedulers = interactorSchedulers;
    }

    public Single<List<String>> getActors(String movieId) {
        return moviesRepository.getMovie(movieId)
                .flatMap(Movie::getActors)
                .subscribeOn(schedulers.getExecutionScheduler())
                .observeOn(schedulers.getPostExecutionScheduler());
    }
}
```

## Repositories – Entities Layer

Up until this moment, you saw that a View works with a Presenter directly. Presenter then calls Interactor directly. But now we apply the Inversion of Control principle and the Interactor works with the interface of the repository which is provided from Dagger and its real implementation can be changed whenever we want.

### CRUD only

For repositories, we follow a simple rule. They are only responsible for CRUD operations on a certain type of data. In the movies example, the MovieApiRepository is responsible for:  
– Getting the list of movie  
– Finding a movie on the list

If you need to get the actors of the movie, you don’t give the responsibility to the repository like Repository.getActorsForMovie. The MovieApiRepository is only responsible for movies, not for actors. It doesn’t make sense, right?

### API Services

In most of the cases, you will find yourself calling the API layer here. It is fine as long as the repository does not know anything about the real implementation of your API service. We work with an interface from Retrofit and that is fine in our case. We can also implement caching here without any change in the code in the rest of the application.

### Network Mappers

Our network service will always return classes which we don’t use directly. That is a rule which helps us escape the dependency on the network layer in the layers below it. So we have classes like MovieJson, ActorJson etc. which are just a representation of the data we receive from the API. If we want to use the data we receive from the network, we use ApiMapper classes to transform this data for us to a meaningful object.

### Example

```
private MoviesApiService apiService;
    private MovieApiMapper movieApiMapper;

    public MoviesApiRepository(MoviesApiService apiService, MovieApiMapper movieApiMapper) {
        this.apiService = apiService;
        this.movieApiMapper = movieApiMapper;
    }

    @Override
    public Single<List<Movie>> getMovies(String search) {
        return apiService.getMovies(search, Constants.API_KEY)
                .doOnSuccess(this::cacheJson)
                .flattenAsObservable(SearchJson::getMovies)
                .map(x -> movieApiMapper.toMovie(x))
                .toList();
    }
}
```

## Business Objects

Transforming a \*Json class to a meaningful class is a simple thing. We just use a mapper and transfer the knowledge inside of it like which fields are needed to construct the new object.

### MovieJson to Movie

But there is an idea which must be erased immediately. Transforming MovieJson to a Movie object which is the exact same representation of the data is a bad idea. We do not want to transfer the same object which is part of a differently named class. We want this object to be smart. In this case, we know that each movie has a list of actors. So it makes sense for the Movie class to have a getActors method. But because we need to make a request to the server to get the list of actors, we need something called a collaborator here.

### Collaborators

Collaborators are classes hidden behind interfaces which are used inside our Business Objects and make them smart. In our case, a Movie has a list of actors that play in it. To get the list of the actors for a given movie we call Movie.getActors(). Then the collaborator is used to get the data for us and return it whenever it is done. This makes our object smarter and prevents us from creating GOD objects that control everything.

### Factories

Because we need the collaborator inside our Movie class, it is not a good idea to pass the collaborator to the MovieApiMapper class as it is a mapper and should not know anything about constructing our business object with something more than a map between fields. That is why we create a MovieFactory that knows about the collaborator and knows how to construct the Movie class. Then the ApiMapper calls the factory to construct a new object. Let’s look at an example.

### Example

```
public interface ActorsService {
    Single<List<String>> getActors(Movie movie);
}

public class ActorsApiService implements ActorsService {
    @Override
    public Single<List<String>> getActors(Movie movie) {
        return Single.just(new ArrayList<String>() {{
            add("Jason Statham");
            add("Vin Diesel");
            add("Silvester Stalone");
        }});
    }
}

public class Movie {
    private final ActorsService actorsService;
    private String title;
    private String id;

    public Movie(ActorsService actorsService, String title, String id) {
        this.title = title;
        this.id = id;
        this.actorsService = actorsService;
    }

    public String getTitle() {
        return title;
    }

    public String getId() {
        return id;
    }

    public Single<List<String>> getActors() {
        return actorsService.getActors(this);
    }
}

public class MovieApiMapper {

    private MovieFactory movieFactory;

    @Inject
    public MovieApiMapper(MovieFactory movieFactory) {
        this.movieFactory = movieFactory;
    }

    public Movie toMovie(MovieJson movieJson) {
        return movieFactory.toMovie(movieJson.getTitle(), movieJson.getImdbID());
    }
}

public class MovieFactory {

    // Only the factory knows about this collaborator
    private ActorsService actorsService;

    @Inject
    public MovieFactory(ActorsService actorsService) {
        this.actorsService = actorsService;
    }

    public Movie toMovie(String title, String id) {
        return new Movie(actorsService, title, id);
    }
}
```

## Testing

With all this separation of concerns it is quite easy to test everything you want. Testing an interactor is the simples one, here is an example:

```
@RunWith(MockitoJUnitRunner.class)
public class GetMoviesInteractorTest {
    private GetMoviesInteractor getMoviesInteractor;

    @Mock
    MoviesRepository moviesRepository;

    @Mock
    Movie movie;

    @Before
    public void setUp() {
        getMoviesInteractor = new GetMoviesInteractor(moviesRepository, new TestSchedulers());
    }

    @Test
    public void returnsMovieFromSearch() throws Exception {
        List<Movie> result = Arrays.asList(movie);
        given(moviesRepository.getMovies(any())).willReturn(Single.just(result));

        TestObserver<List<Movie>> testObserver = getMoviesInteractor.getMovies("Test").test();

        testObserver.awaitTerminalEvent();
        testObserver.assertResult(result);
    }
}
```

You just mock the repository & the schedulers and you are good to go. The same thing holds true for the Presenter & the Repository. You mock either the View or the API, the mappers and you are good to go. All of this boilerplate code really plays off when you start to test your code.

## Other Topics

### Item Selection

Item selection is an interesting topic. What we do in our team is we use a Repository to store the selected item and then the new screen that loads calls the Repository to get the selected item from there. This way we remove the dependency on the ID of the item and also we don’t pass any item ids between screens.

Pros:  
– No need to pass ids through bundles  
– Selected item can be accessed from different screens easily

Cons:  
– More boilerplate code  
– Selection is made inside a repo which is a singleton

In the current example, I preferred passing the id of the selected movie because it is simpler for me. I would prefer to not do the overhead which comes from having an additional interactor & repository layer for storing and retrieving selection.

### Scopes

In the Movies example, everything is part of the Activity & Application components. If you want, you can have additional layers based on either Application/Activity/Fragment or LoggedIn/LoggedOut/MainList scopes. It is a personal preference how you implement the scope. The main thing is to have Repositories as singletons as they should store the data no matter the fragment or activity lifecycle.

Considering the impact of repositories in memory on the device should be your last topic and it should be measured. Making simple assumptions may lead you to make the wrong choice.

### TargetFragment

If you have DialogFragments – keep them as simple as possible. No RxJava calls, nothing. Just return the result to the caller by using setTargetFragment. The caller is responsible for managing the network calls etc. There were many cases where guys from the team would make separate presenters and make API calls inside of the dialog. But you know that the dialog is removed immediately when the user choses an action from it. So `stopPresenting` would immediately be called and no network calls would happen. So keep the DialogFragment as simple as possible.

### Adapter Click Listener

Don’t pass a Presenter to the Adapter. Adapter should not know about it. Instead expose an OnClickListener from the Adapter and let the View call the presenter. This is the best way to make this adapter reusable without having to pass the same presenter again and again.

### Github

You can download the sources of the project [HERE](https://github.com/n0m0r3pa1n/android-clean-videos).
