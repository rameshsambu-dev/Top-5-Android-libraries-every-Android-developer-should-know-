# Top-5-Android-libraries-every-Android-developer-should-know-
Top 5 Android libraries every Android developer should know 

In the last year or so, Android development has really come of age. Android Studio with Gradle at its core is a dash of light after Eclipse. Besides that, there are quite a few open source libraries that we use on a daily basis.

Here is a selection of five of our favorite ones and a list of links where you can find others.


1. RETROFIT
From their site: "Retrofit turns your REST API into a Java interface.” It’s an elegant solution for organizing API calls in a project. The request method and relative URL are added with an annotation, which makes code clean and simple.

With annotations, you can easily add a request body, manipulate the URL or headers and add query parameters.

Adding a return type to a method will make it synchronous, while adding a Callback will allow it to finish asynchronously with success or failure.

2.GSON

Gson is a Java library used for serializing and deserializing Java objects from and into JSON. A task you will frequently need to do if you communicate with APIs. We mostly use JSON because it’s lightweight and much simpler than XML.

// Serialize 
String userJSON = new Gson().toJson(user);

// Deserialize
User user = new Gson().fromJson(userJSON, User.class);
It also plays nice with the next library:


3. EVENTBUS
EventBus is a library that simplifies communication between different parts of your application. For example, sending something from an Activity to a running Service, or easy interaction between fragments. Here is an example we use if the Internet connection is lost, showing how to notify an activity:

public class NetworkStateReceiver extends BroadcastReceiver {

    // post event if there is no Internet connection
    public void onReceive(Context context, Intent intent) {
        super.onReceive(context, intent);
        if(intent.getExtras()!=null) {
            NetworkInfo ni=(NetworkInfo) intent.getExtras().get(ConnectivityManager.EXTRA_NETWORK_INFO);
            if(ni!=null && ni.getState()==NetworkInfo.State.CONNECTED) {
                // there is Internet connection
            } else if(intent
                .getBooleanExtra(ConnectivityManager.EXTRA_NO_CONNECTIVITY,Boolean.FALSE)) {
                // no Internet connection, send network state changed
                EventBus.getDefault().post(new NetworkStateChanged(false));
            }
}

// event
public class NetworkStateChanged {

    private mIsInternetConnected;

    public NetworkStateChanged(boolean isInternetConnected) {
        this.mIsInternetConnected = isInternetConnected;
    }

    public boolean isInternetConnected() {
        return this.mIsInternetConnected;
    }
}

public class HomeActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EventBus.getDefault().register(this); // register EventBus
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        EventBus.getDefault().unregister(this); // unregister EventBus
    }

    // method that will be called when someone posts an event NetworkStateChanged
    public void onEventMainThread(NetworkStateChanged event) {
        if (!event.isInternetConnected()) {
            Toast.makeText(this, "No Internet connection!", Toast.LENGTH_SHORT).show();
        }
    }

}



4.RxJava

RxJava – Reactive Extensions for the JVM – a library for composing asynchronous and event-based programs using observable sequences for the Java VM.


5.Glide

An image loading and caching library for Android focused on smooth scrolling https://bumptech.github.io/glide/
