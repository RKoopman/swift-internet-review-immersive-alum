### Internet Review Doc

### Overview
What do I need to know.

1. How to make a URL Object
2. How to make a Session
3. How to make a Data Task 
4. How to parse JSON


#### 1. How to make a URL Object

```
var urlString = "http://api.someurl.com"
var url = URL(string:urlString)

//Voila I have a URL



```
![](https://s3.amazonaws.com/learn-verified/swift-UrlObject.png)

#### How to make a Session

```
var session = URLSession.shared
```



#### How to make a Data Task
DataTasks must be created using the session object and require a URL (or Request)

However before we get to the dataTask we need to unwrap our url
 
 
You should have up to here in your code
 
 
```
let urlString = "http://api.someurl.com"
let url = URL(string:urlString)
let session = URLSession.shared
```

You now have one or two options

1 .

```
let urlString = "http://api.someurl.com"
let url = URL(string:urlString)
let session = URLSession.shared
if let unwrappedURL = url{
  
}
```
2.

```
let urlString = "http://api.someurl.com"
let url = URL(string:urlString)
let session = URLSession.shared
guard let unwrappedUrl = url else { return }

```

Now that we have unwrapped the url we can create a dataTask using the session object 

1. Create a task variable that uses the session.dataTask(with:, completionHandler:) method

![](https://s3.amazonaws.com/learn-verified/dataTask1.png)

2. Enter your URL object and use the autocomplete to enter variables for the closure

![](https://s3.amazonaws.com/learn-verified/dataTask2.png)

![](https://s3.amazonaws.com/learn-verified/dataTask3.png)

Convention dictates we name these variables data, response and error. You can give them different names if you (I don't recommend it) but remember the order is important

3. Your code should now look like the following

```
let urlString = "http://api.someurl.com"
let url = URL(string:urlString)
let session = URLSession.shared
guard let unwrappedUrl = url else { return }
let task = session.dataTask(with: unwrappedUrl, completionHandler: { (data, response, error) in

                
 })

```

The final procedure for using a task is to resume the task. If you do not call resume the task will not run. 

```

let urlString = "http://api.someurl.com"
let url = URL(string:urlString)
let session = URLSession.shared
guard let unwrappedUrl = url else { return }
let task = session.dataTask(with: unwrappedUrl, completionHandler: { (data, response, error) in

                
 })
task.resume()

```

#### How to parse JSON

JSON can be obtained from the data sent back from the server. We will be using the data variable to create some JSON. Data is an optional value so we must first unwrap the data.

```
let task = session.dataTask(with: unwrappedUrl) { (data, response, error) in
                if let unwrappedData = data{
                    
                }
            }
```

Data can be converted to JSON using JSONSerialization. 

```
var responseJSON = JSONSerialization.jsonObject(with: unwrappedData, options: [])

```
The line above creates a jsonObject out of our data. This jsonObject on its own is useless since we are more familiar using Dictionaries and Arrays. As a result we need to convert our jsonObject to a Dictionary or Array. We do this by casting it with [String:Any] for a Dictionary and a [[String:Any]] for an Array. Remember that JSON can be in either form and we must check with Postman first

```
let task = session.dataTask(with: unwrappedUrl) { (data, response, error) in
                if let unwrappedData = data{
                    var responseJSON = JSONSerialization.jsonObject(with: unwrappedData, options: []) as! [String:Any]
                    
                }
            }
```

###But wait we have an error

JSONSerialization.jsonObject(with:) was written in such a way that it must be marked with try. This is solved using the Do, Try, Catch pattern.

![](https://s3.amazonaws.com/learn-verified/dataTask4.png)

Once we add Do, Try and Catch this error goes away

![](https://s3.amazonaws.com/learn-verified/dataTask5.png)


Your code should now look like this 

```
let urlString = "http://api.someurl.com"
    let url = URL(string:urlString)
    let session = URLSession.shared
    guard let unwrappedUrl = url else { return }
    let task = session.dataTask(with: unwrappedUrl, completionHandler: { (data, response, error) in
        if let unwrappedData = data{
            do{
                let responseJSON = try JSONSerialization.jsonObject(with: unwrappedData, options: []) as! [String: Any]
            }catch{
                
            }
            
        }
        
    })
    task.resume()
```

### Conclusion

Using the internet takes practice. Reach out to the instructors if you are interested in playing around with different APIs. There is some copy paste code at the bottom

### Things I Don't need for the assessment but should be aware of 

1. How to do different HTTP verbs
2. How to get status code
3. How to send a header
4. How to use a queryString/Parameters



### Some Copy Paste Code

```

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        
        let urlString = "https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY"
        
        let url = URL(string: urlString)
        
        
        if let unwrappedUrl = url{
            let session = URLSession.shared
            let task = session.dataTask(with: unwrappedUrl) { (data, response, error) in
                if let unwrappedData = data{
                    do {
                        let responseJSON = try JSONSerialization.jsonObject(with: unwrappedData, options: []) as! [String:Any]
                        
                        print(responseJSON)
                        
                    }catch{
                        
                    }
                    
                    
                }
            }
            task.resume()
            
        }
        
    }

}


```









































