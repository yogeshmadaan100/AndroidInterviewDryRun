# AndroidInterviewDryRun
This repo contains the answer of Android Dry Run Interview under Udacity Android Nanodegree Program.
#Questions
Question 1 - What’s your favorite tool or library for Android? Why is it so useful?

Answer - The most favourite library is Rx android - android version of RX Java that help us to solve problems using reactive programming techniques. The most interesting part of the library is that it helps in reducing most of the hectic work of developer ranging from api calls implementation, thread management, loaders, synchronizations and many more. Moreover the overall architecture encouraged by the library helps us to have abstraction over the mdoels and Ui layer following the proper MVC pattern and thus helps in developing a scalabe and reliable product.

Question 2 - You want to open a map app from an app that you’re building. The address, city, state, and ZIP code are provided by the user. What steps are involved in sending that data to a map app?

Answer - The code snippet for above problem is 

public void getLocationOnMap(String address, String city, String state, String zip)

{

  Uri gmmIntentUri = Uri.parse("geo:0,0?q="+address+", "+city+" , "+ state +" , " +zip);
  
  Intent mapIntent = new Intent(Intent.ACTION_VIEW, gmmIntentUri);
  
  mapIntent.setPackage("com.google.android.apps.maps");
  
  if (mapIntent.resolveActivity(getPackageManager()) != null) {
  
    startActivity(mapIntent);
    
  }
  
}

Question 3 - Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. The method signature is: “public static String compress(String input)” You must write all code in proper Java, and please include import statements for any libraries you use.

Answer - 


private static String compressString(String str) {

        if (str == null) {
        
            return null;
            
        } else if (str.length() <= 2) {
        
            return str;
            
        }
        
 
        // using String type will make the Time Complexity to be O(n^2)
        // because the String in java is immutable, and it will create a new string if you modify it.
        StringBuilder stringBuilder = new StringBuilder();
        char temp = str.charAt(0);
        int count = 1;
        for (int i = 1; i < str.length(); i++) {
            if (temp == str.charAt(i)) {
                count++;
            } else {
                stringBuilder.append(temp);
                stringBuilder.append(count);
                temp = str.charAt(i);
                count = 1;
                // once the compress string's length > input string, just return the original input string
                if (stringBuilder.length() >= str.length()) {
                    return str;
                }
            }
        }
        // do not forget appending the last character and count to stringBuilder!!!
        stringBuilder.append(temp).append(count);
 
        // Compare the length with the original one, return the less length string type.
        if (stringBuilder.length() >= str.length()) {
            return str;
        } else {
            return stringBuilder.toString();
        }
    }
    

Question 4 - List and explain the differences between four different options you have for saving data while making an Android app. Pick one, and explain (without code) how you would implement it.

Answer - 
Android provides several options for you to save persistent application data. 

Available data storage options are :

* Shared Preferences - Store private primitive data in key-value pairs.
* Internal Storage - Store private data on the device memory.
* External Storage - Store public data on the shared external storage.
* SQLite Databases - Store structured data in a private database.
* Network Connection - Store data on the web with your own network server.

Shared Preferences

The SharedPreferences class provides a general framework that allows you to save and retrieve persistent key-value pairs of primitive data types. You can use SharedPreferences to save any primitive data: booleans, floats, ints, longs, and strings. This data will persist across user sessions (even if your application is killed).

User Preferences

Shared preferences are not strictly for saving "user preferences," such as what ringtone a user has chosen. If you're interested in creating user preferences for your application, see PreferenceActivity, which provides an Activity framework for you to create user preferences, which will be automatically persisted (using shared preferences).
To get a SharedPreferences object for your application, use one of two methods:

getSharedPreferences() - Use this if you need multiple preferences files identified by name, which you specify with the first parameter.

getPreferences() - Use this if you need only one preferences file for your Activity. Because this will be the only preferences file for your Activity, you don't supply a name.

To write values:
* Call edit() to get a SharedPreferences.Editor.
* Add values with methods such as putBoolean() and putString().
* Commit the new values with commit()

To read values:

Use SharedPreferences methods such as getBoolean() and getString().

Question 5 - Given a directed graph, design an algorithm to find out whether there is a route between two nodes. The method signature is: “public static < T > boolean findPath(GraphNode< T > start, GraphNode< T > end)”. Assume you have a “GraphNode” class that has a getChildren() method, which returns a List< GraphNode< T >> object. You must write all code in proper Java, and please include import statements for any libraries you use (no need to import GraphNode).

Answer - 

class Graph
{
    private int V;   // No. of vertices
    private LinkedList<Integer> adj[]; //Adjacency List
 
    //Constructor
    Graph(int v)
    {
        V = v;
        adj = new LinkedList[v];
        for (int i=0; i<v; ++i)
            adj[i] = new LinkedList();
    }
 
    //Function to add an edge into the graph
    void addEdge(int v,int w)  {   adj[v].add(w);   }
 
    //prints BFS traversal from a given source s
    Boolean isReachable(int s, int d)
    {
        LinkedList<Integer>temp;
 
        // Mark all the vertices as not visited(By default set
        // as false)
        boolean visited[] = new boolean[V];
 
        // Create a queue for BFS
        LinkedList<Integer> queue = new LinkedList<Integer>();
 
        // Mark the current node as visited and enqueue it
        visited[s]=true;
        queue.add(s);
 
        // 'i' will be used to get all adjacent vertices of a vertex
        Iterator<Integer> i;
        while (queue.size()!=0)
        {
            // Dequeue a vertex from queue and print it
            s = queue.poll();
 
            int n;
            i = adj[s].listIterator();
 
            // Get all adjacent vertices of the dequeued vertex s
            // If a adjacent has not been visited, then mark it
            // visited and enqueue it
            while (i.hasNext())
            {
                n = i.next();
 
                // If this adjacent node is the destination node,
                // then return true
                if (n==d)
                    return true;
 
                // Else, continue to do BFS
                if (!visited[n])
                {
                    visited[n] = true;
                    queue.add(n);
                }
            }
        }
 
        // If BFS is complete without visited d
        return false;
    }
 
    // Driver method
    public static void main(String args[])
    {
        // Create a graph given in the above diagram
        Graph g = new Graph(4);
        g.addEdge(0, 1);
        g.addEdge(0, 2);
        g.addEdge(1, 2);
        g.addEdge(2, 0);
        g.addEdge(2, 3);
        g.addEdge(3, 3);
 
        int u = 1;
        int v = 3;
        if (g.isReachable(u, v))
            System.out.println("There is a path from " + u +" to " + v);
        else
            System.out.println("There is no path from " + u +" to " + v);;
 
        u = 3;
        v = 1;
        if (g.isReachable(u, v))
            System.out.println("There is a path from " + u +" to " + v);
        else
            System.out.println("There is no path from " + u +" to " + v);;
    }
}

Question 6 - What are your thoughts about Fragments? Do you like or hate them? Why?

Answer - A Fragment represents a behavior or a portion of user interface in an Activity. You can combine multiple fragments in a single activity to build a multi-pane UI and reuse a fragment in multiple activities. 

The main advantages of useing fragments are 
* It helps in designing for different screen sizes.
* Data transfer between fragments are easier as compared to activities.
* Better user expererience and organisation.
* Supports advances user metaphors.



