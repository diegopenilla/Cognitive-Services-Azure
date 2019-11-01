![](https://miro.medium.com/v2/resize:fit:4800/format:webp/0*lR2uBXtN6z3I0WFx)
# AI in the Cloud with Python

> Create a simple bot with vision, hearing, and speech using Azure and Colab Notebooks

There is a fleet of clouds with their own minds floating over the internet trying to take control of the winds. They’ve been pushing very aggressively all kinds of services into the world and absorbed data from every possible source. Among this big bubble of services, an increasing number of companies and applications are relying on pre-made AI resources to extract insights, predict outcomes and gain value out of unexplored information. If you are wondering how to try them out, I’d like to give you an informal overview of what you can expect from sending different types of data to these services. In short, we’ll be sending images, text and audio files high into the clouds and explore what we get back.

While this way of using AI doesn’t give you direct, full control over what’s happening (as you would using machine learning frameworks) its a quick way for you to play around with several kinds of models and use them in your applications. It’s also a nice way to get to know what’s already out there.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*1qlmc54vLWIb6ACI)
<div align='center'>
<p>
<a href="https://miro.medium.com/v2/resize:fit:720/format:webp/0*1qlmc54vLWIb6ACI"> Source </a>
</p>
</div>

In general, before we can use any kind of cloud service, we must first:

- Create a subscription with a particular cloud provider.
- Create a resource: to register the particular service we’ll be using and
- Retrieve credentials: to authorize our applications to access the service.

And while there are many cloud providers likely able to suit your needs, we’ll be looking at Microsoft’s Azure Cloud. There is a ton of options and tutorials that will likely confuse you if you don’t know where to start, so for the first part of this post we will walk through from scratch and get to know what we need to make use of the following services:

- [Computer Vision](https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/)
- [Face](https://azure.microsoft.com/en-us/services/cognitive-services/face/)
- [Text Analytics](https://azure.microsoft.com/en-in/services/cognitive-services/text-analytics/)
- [Speech Services](https://azure.microsoft.com/en-in/services/cognitive-services/speech-services/)
- [LUIS](https://www.luis.ai/home)
  
All resources from the Cognitive Services platform of Azure, a nice collection of services with use cases in the areas of vision, speech, language, web search, and decision. In their words:

> The goal of Azure Cognitive Services is to help developers create applications that can see, hear, speak, understand, and even begin to reason.

We will put them in action using a Colab notebook: where we will set up everything we need in Azure, implement code to call these services and explore the results. To make it more fun, we will also make use of the camera, microphone and speakers to speak, see and hear responses from the cloud!

## Setup

All you need to give life to the notebook is an Azure subscription. After that, you should be able to run the examples in this post without trouble.

### Create a subscription

Create an Azure subscription following this link. (If you are currently enrolled in a university, use this link). If it is your first account you should have some trial money to spend, but make sure you always check the prices before using anything. For this tutorial, we’ll be only using free services, but it’s still a good thing to do!

After you have made an account, you will have access to the Portal. Here you can manage everything about your subscription and configure huge amounts of stuff.

![](https://miro.medium.com/v2/resize:fit:640/format:webp/1*9aSKEFP6GXfg0qSlwFc_kw.png)

The portal is a very interesting place, there are entire companies managed using this [big pack of tools](https://azure.microsoft.com/en-us/solutions/). To not get lost in the woods, I implemented code to set up all you will need for this post inside the [notebook](https://colab.research.google.com/drive/1BIq7Ll7lFwnEH5Dwd2_KUlKTyqBvLdxY). But I’ll take time here to explain the basics and give you an idea of how to do it yourself.

But if you already know how to create groups and resources, feel free to skip entirely the *Setup in Azure* section below and jump straight into the [notebook](https://colab.research.google.com/drive/1BIq7Ll7lFwnEH5Dwd2_KUlKTyqBvLdxY).

## Setup in Azure

This is all automated in the notebook but read through if you want to know how to do it yourself.

## Creating a resource group

Before you can create specific resources (e.g. Computer Vision or Text Translation), you need to create a *group* to hold multiple resources together. In other words, each resource we create must belong to a group. This allows you to manage all of them as an entity and keep track of things more easily.

In general, there are two main ways to get stuff done in the cloud: you can either use the graphical user interface (GUI) of the cloud providers (e.g the Portal of Azure) or type lines of code in a command-line interface (CLI). To make this clear, let’s see how to create a resource group in both ways:

- To create a resource group using the portal:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*SR7WUHD3vJZEehtUeBBg9A.gif)


On the left menu, go to “Resource groups”, click on “Add” and fill in the “Resource group” and “Region” fields with a name and a location.

*Note*: for the entire tutorial we will be using MyGroup as our resource group name and West Europe as our region. The location argument specifies which region you want the data center/services to be located. If you are not in west Europe besides a bit of latency it shouldn’t change much for you. Although it could be simple to change to another region, the argument will come over and over again when making use the resources, (often under different names) so if you are setting things up by yourself, useMyGroup and WestEurope to keep things simple and allow you to run the code examples without changes later on.

- To achieve exactly the same and create myGroup without having to use the GUI in the Portal, we could have also used the command-line interface. By clicking on >_ in the top menu in your portal:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*xhWuj3CV9RPfXsfVpgcXgw.png)

And typing this in the window that pops up:

```bash
az group create -l westeurope -n MyGroup
```

Just as before, this will make a resource group named MyGroup in the location westeurope.

What you see clicking on the >_ icon in the portal is the command line of a virtual machine they provide for you. It is really powerful and comes with very handy tools: its own environments for go, python and other languages, docker, git, etc. ready to go. But most importantly it comes with az a command-line tool from Azure that provides options to do basically all you see in the portal and more. This is the tool we use inside the notebook to set up everything.

The az tool is way too much to cover here but feel free to type az and start exploring!

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*MAUo0zoKU4tTFFcodoavcw.png)

## Creating a Resource

Let’s now create individual resources and assign them to the group MyGroupthat we made. As before, we can either use the GUI or the CLI. For intuition’s sake: this is how the whole the process goes using the GUI:

- Steps to create a resource in the Portal:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*AyOGhD3MDl6EaW22ngRgkw.gif)

- On the left menu, click on “Create a resource”, search for the one you want (e.g. Computer Vision) and fill in the project details.
- In “Location”, specify which region you want the data center/services to be located. (We will be using ‘West Europe’ ).
- The “Pricing tier” of each resource defines its cost. For each resource and tier, there will be different prices and conditions. For this tutorial, we always select the free tier F0 to avoid any charges.

- Finally, we assign our resource to an existing resource group (MyGroup ).
  
After the resource has been made, we need to retrieve its key to be able to use it. The key is the credential that will authorize our application to use the service. This is how you retrieve a key in the Portal:

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*Eh4mbPqoCmmqT0uBhkAZjg.gif)

- On the left menu, go to “All resources” and click on top of your target resource. Under “Resource Management”, go to “Keys” and write down the credential.


Briefly explained, but that’s how you create resources and retrieve keys manually in the Portal. I hope this gives you an intuition of how to do it. But following this procedure for all the resources we need would take a while. So to relieve the pain and prevent readers from getting bored. I made a snippet to set up everything at once using the az tool in the CLI. The snippet will create and retrieve the keys for the following resources, (specifying the free tier F0):

- [Computer Vision](https://azure.microsoft.com/en-in/services/cognitive-services/computer-vision/)
- [Face](https://azure.microsoft.com/en-us/services/cognitive-services/face/)
- [LUIS: Language Understanding](https://www.luis.ai/home)
- [Text Analytics](https://azure.microsoft.com/en-in/services/cognitive-services/text-analytics/)
-[SpeechServices](https://azure.microsoft.com/en-in/services/cognitive-services/speech-services/)

Go the command line in the portal (or install az in your machine):

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*xhWuj3CV9RPfXsfVpgcXgw.png)

Then copy, paste and run this snippet:

```bash
# create array with all the resources
resources=( 
"ComputerVision"
"Face"
"LUIS"
"TextAnalytics"
"SpeechServices")

# loop over resources and create them
for i in "${resources[@]}"; do
    echo Creating Resource for $i
    echo $(az cognitiveservices account create --name $i --resource-group MyGroup \
           --kind $i --sku F0 --location WestEurope --yes)
done

# creating file where to save the results
touch keys.py

echo "subscriptions = {" >> keys.py
# for every resource find the key and append it to keys.py
for i in "${resources[@]}"; do
    echo "Retrieving Key for Resource $i "
    echo -e "\t\"$i\"": $(az cognitiveservices account keys list --name $i \
         --resource-group "MyGroup" | grep key1 | cut -d' ' -f 4) >> keys.py
done && echo "}" >> keys.py && echo "print(subscriptions)" >> keys.py

# printing results
python keys.py
```

- We create an array resources with the specific names of the arguments we want.
- We iterate over resources and apply the commandaz cognitive services account create to create each service individually. (Here, we specify the location WestEurope, the free tier F0 and the resource group we made MyGroup).
- We loop again over resources and apply the command az cognitiveservices account keys to retrieve the keys for each resource and append them into a file called keys.py.
- When this has finished running. keys.py should contain a standard dictionary with the resources and their credentials:


```python
# sample keys.py with fake credentials
subscriptions = {
  'TextAnalytics': 'ec96608413easdfe4ad681',
  'LUIS': '5d77e2d0eeef4bef8basd9985',
  'Face': '78987cff4316462sdfa8af',
  'SpeechServices': 'f1692bb6desae84d84af40',
  'ComputerVision': 'a28c6ee267884sdff889be3'
  }
```

All we need to give life to our [notebook](https://colab.research.google.com/drive/1BIq7Ll7lFwnEH5Dwd2_KUlKTyqBvLdxY) :). You can verify and see what we did by typing:

```bash
az resource list 
```
Alright! If you made it so far, I’ll reward you with a collection of snippets to call and send content to these resources.

## Accessing Cognitive Services

Now that we’ve been through the boring stuff, we are ready to use the resources and see what they are capable of. Creating resources has given us access to use their respective REST-APIs. There is a whole lot we can do with what we just made.

In short, to access these services we’ll be sending requests with certain parameters and our content to specific URLs, trigger an action on the server and get back a response. To know how to structure our requests for each of the services we’ll be using we need to refer to the API docs (here is where you can really appreciate what the API is useful for).

Each API has a collection of functions ready to use. For example, with the [Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa), we can perform OCR (Optical Character Recognition) and extract text from images, describe images with words, detect objects, landmarks and more.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*3VS2OStN0p7wpg4eeY1Ulg.png)

It might feel overwhelming to look up the docs for all the APIs we have access to:

- [Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc)
- [Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Text Analytics API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1/operations/5ac4251d5b4ccd1554da7634)
- [LUIS API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f)
- [SpeechServices API](https://docs.microsoft.com/en-gb/azure/cognitive-services/speech-service/rest-speech-to-text)
- 
So let’s go over an example to give you an intuition of what you do to get something started. Assume you are looking up the “Analyze Image” method of the Computer Vision API.

- Go to the API docs, select the method you want (“Analyze Image”) and scroll down :O!
- You see a URL where to send your requests and with which parameters.
- What the request’s header and body are made of.
- An example response from the server.
- Error responses and explanation.
- And code samples for many languages including Python. Where you just need to replace the resource’s key and the data to be sent. This will become clear once you see it implemented.

### Sending a request

The APIs from different Cognitive Services resemble each other a lot! If you go through the code samples, you will notice that they all share the same backbone, but are just directed to different URLs. This is basically how the backbone of our requests looks like:

```python
# Part 1: Define, Request headers, body and parameters ###########################

headers = {
    'Content-Type': '{data type}', # e.g. application/json, application/octet-stream
    'Ocp-Apim-Subscription-Key': '{resource key}', # for the specific resource
}

# body with our content, depends on Content-Type for example:
body = {"url":"http://<www.url_to_data>"} 


# Request parameters that are specific to each API and method
params = urllib.parse.urlencode({
  # ...
})

# Part 2: Send params, body and header to the server ##########################
try:
    conn = http.client.HTTPSConnection('<location>.api.cognitive.microsoft.com')
    # send a POST request with headers, body and parameters to a certain URL 
    conn.request("POST", "/<api>/<version>/<method>?%s" % params, body, headers)
    response = conn.getresponse()
    # save response
    data = response.read()
    conn.close()
    print(data)
except Exception as e:
    print("[Errno {0}] {1}".format(e.errno, e.strerror)
```

In short, we define headers , body and params (request parameters), send them to a certain URL and receive a response.

- In the `headers` we specify the type of data we want to send and the resource key to access the API.
- In thebody we include (or point) to the data we want to send. The `body` itself can have many forms and depends on the API.
- In the `params`` we specify the (often optional) parameters to tell the API more specifically what we want.
- We then send these variables as a standard request to a specific endpoint, for example: `westeurope.api.cognitive.microsoft.com/computervision/v2.0/describe`

In the notebook, we take advantage of these common elements to implement two utility functions:get_headers_body and send_request to help us makeup requests more quickly and avoid repeating too much code.

Let’s now get our hands dirty! Jump into the Colab notebook. I’ve put there additional code to `take_picture` , `show_picture`, `record_audio`, `play_audio` and more. These will be the effectors and actuators in our notebook and will allow us to interact with the cloud.

We are not gonna cover all that’s possible with each API but will simply look at a couple of methods and how to call them.

For each API, we will define a couple of functions and see practical examples of how to use them. The responses from the APIs often contain a lot of information! We will be **parsing these responses** and returning only a small part of it (we will not look at the full responses).

## Computer Vision API

> Processes images and returns various information.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*DrbJYxYjEEFZydMX)
<div align='center'>
<p>
<a href="https://unsplash.com/?utm_source=medium&utm_medium=referral"> Source </a>
</p>
</div>

Let’s define a set of functions that will return visual information about images. The functions will make up our requests based on the image we want to analyze and send them to specific URLs (endpoints) of the Computer Vision API.

Here we will make use of the functions `get_headers_body` and `send_request` to speed things up (check the definition of these and more information on the notebook).

- `describe`: returns a visual descriptions about an image.

```python3
def describe(img, number_of_descriptions=1, localfile=False):
    '''Returns a number_of_descriptions from an image'''
    params = urllib.parse.urlencode({
    # Request parameters
    'maxCandidates': "{}".format(number_of_descriptions),
    'language': 'en'})
    
    # Defining body and headers
    headers, body = get_headers_body(API='ComputerVision', data=img, localfile=localfile)
    
    # Send headers, body a params and retrieve respose
    r = send_request('/vision/v2.0/describe', params=params, headers=headers, body=body)
    
    # Parse response to return the info of interest
    description = '\n'
    for i in range(len(r['description']['captions'])):
      description += r['description']['captions'][i]['text'].capitalize()+'\n'
    return description
    if hasattr(img, 'close'):
      img.close()
```

Let's see it in action:

```python3
describe(source, number_of_descriptions=3)
```
![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*3VmkO2axQ1d2b_QJ)

```
A yellow toy car 
A close up of a toy car 
A yellow and black toy car
```

Not bad!

- `classify`: assigns a category to an image and tags it.

```python
def classify(img, localfile=False, visualFeatures='Description, Categories, Faces'):
    '''visualFeatures: a string of comma separated keywords 'Brands, Adult, Objects'
    Returns tags in an image
    '''
    params = urllib.parse.urlencode({
      # Request parameters
      'visualFeatures': '{}'.format(visualFeatures),
      'details': '',
      'language': 'en'})
    
    # Defining body and headers
    headers, body = get_headers_body(API='ComputerVision', data=img, localfile=localfile)
    # Send headers, body a params and retrieve respose
    r = send_request("/vision/v2.0/analyze", params=params, headers=headers, body=body)
    
    # Parse response to return the info of interest
    info = '\nCategories in Image: \n'
    for i in r['categories']:
      info+=("\t"+ i['name'] +'\n')
    info+='Tags found in the image \n\t {}'.format(r['description']['tags'])
    return info
    if hasattr(img, 'close'):
      img.close()
```

```
classify(source)
```

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*HnKXGdtiDpapoeGR)

```bash
Categories in Image: outdoor_stonerock Tags found in the image:['outdoor', 'nature', 'mountain', 'man', 'water', 'waterfall', 'riding', 'going', 'hill', 'snow', 'covered', 'skiing', 'forest', 'large', 'lake', 'traveling', 'river', 'slope', 'standing', 'white', 'wave']
```

`read` : performs optical character recognition and extracts text in an image. Draws regions where text is located and displays the result.

```python
def read(img, localfile=False):
  '''Retrieves text from images using OCR'''
  global draw_show_boxes
  # Request parameters
  params = urllib.parse.urlencode({
      'language': 'unk',
      'detectOrientation': 'true'})
  
  # Defining body and headers
  headers, body = get_headers_body(API='ComputerVision', data=img, localfile=localfile)
  
  # Send headers, body a params and retrieve respose
  r = send_request('/vision/v2.0/ocr', params=params, headers=headers, body=body)
  
  # Parse response to return the info of interest: 
  boxes, texts = [], []# regions in the image
  for i in r['regions']:
    boxes.append([int(i) for i in i['boundingBox'].split(',')])
    text = ''
    for a in i['lines']:
      text+=(' '.join([w['text'] for w in (a['words'])])+'\n')
    texts.append(text)

  # draws rectangles surrounding the objects and info
  IDs = ['Region'+str(i) for i in range(len(boxes))]
  draw_show_boxes(img, IDs, boxes, localfile)
  return texts
```
Besides retrieving the response and printing the extracted text, inside of read we use additional OCR information to draw and label the bounding boxes of detected textual regions in the image. Let’s see it in action:

```python
text = read(source)
for line in text:
    print(line)
```

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*ownB8Cm-gsTaBC9YDasaAg.png)

```
ANALYSIS 
Per Quantitatum 
SERIES, FLUXIONES5 
DIFFERENTIAS. 
c UM 
Enumeratione Linearum 
TERTII ORDINIS. 
LONDI,VIS 
Ex Offcina M.DCC.XL
```
- `see` : returns objects recognized in an image and displays their bounding boxes in the image.

```python
def see(img, localfile=False):
    '''Object Detection, displays the labeled bounding boxes in the image'''    
    # Defining params, body and headers, send and retrieve response
    params = urllib.parse.urlencode({})
    headers, body = get_headers_body(API='ComputerVision', data=img, localfile=localfile)
    r = send_request('/vision/v2.0/detect', params=params, headers=headers, body=body)
    # manipulate response to extract useful information
    print(f"In the image of size {r['metadata']['width']} by {r['metadata']['height']} pixels, {len(r['objects'])} objects were detected")
                                                                               
    found_objects, object_boxes = [], []
    for i in r['objects']:
        found_objects.append(i['object'])
        print('{} detected at region {}'.format(i['object'], i['rectangle']))
        box = []
        for el in 'xywh':
          box.append(i['rectangle'][el])
        object_boxes.append(box)

    # In this case, the response returns the recognized objects and their boundaring boxes.
    # We can draw them in the image with the following function:
    draw_show_boxes(img, found_objects, object_boxes, localfile)
    return found_objects
```

```bash
see(source)
```
![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*lCnyylKw3_uqtWiWgM1oUg.png)

```bash
In the image of size 800 by 1181 pixels, 2 objects were detected 
person detected at region {'x': 354, 'y': 535, 'w': 106, 'h': 280} 
Toy detected at region {'x': 214, 'y': 887, 'w': 186, 'h': 207}
```

### Face API

> Detect, recognize, and analyze human faces in images.

For brevity, we’ll be looking at only one API method:

`detect_face`: shows bounding boxes of faces recognized in an image and some information about them (age, sex, and sentiment).

```python
def detect_face(img, localfile=False):
    """Finds location and attributes of human faces in a picture (age, gender, emotion) and displays them"""
    # Define headers, body and params
    params = urllib.parse.urlencode({
        'returnFaceId': 'true',
        'returnFaceLandmarks': 'false',
        'returnFaceAttributes': 'age,gender,smile,facialHair,glasses,emotion,hair',
        'recognitionModel': 'recognition_01',
        'returnRecognitionModel': 'false'})
    headers, body = get_headers_body(API='Face', data=img, localfile=localfile)
    r = send_request('/face/v1.0/detect', params=params, headers=headers, body=body)
    # extracting information _ _ _ _ _ _ _ _ _ _
    boxes,genders,ages,emotions = [], [], [], []
    for i in r:
      boxes.append([i['faceRectangle'][el] for el in ['left', 'top', 'width', 'height']]) 
      genders.append(' '+i['faceAttributes']['gender'].capitalize())
      ages.append(int(i['faceAttributes']['age']))
      emotions = []
      for i in r:
        e, maxx = None, 0
        for key, val in i['faceAttributes']['emotion'].items():
          if val > maxx:
            e, maxx = key, int(val*100)
        emotions.append(f'{maxx}% {e}')
    print(f'{len(boxes)} faces recognized')

    # Draws object and writes information _ _ _ _ _ _ _
    def draw_show_boxes(img, boxes, genders, ages, emotions, localfile=False):
      if not localfile: # We need to download the image
        r = requests.get(img, allow_redirects=True)
        img = 'faces.jpg'
        open(img, 'wb').write(r.content)
      im = Image.open(img)   
      # Drawing boxes
      d = ImageDraw.Draw(im)
      for i, gender, age, emotion in zip(boxes, genders, ages, emotions):
        # represent the x-coordinate of the left edge, the y-coordinate of the top edge, width, and height of the bounding box
        x, y, width, height = i
        top, top_right = (x, y), (x+width, y)
        bottom, bottom_right = (x, y+height), (x+width, y+height)
        line_color = (0, 0, 255)
        d.line([top, top_right, bottom_right, bottom, top], fill=line_color, width=2)
        d.text(top,f'{gender} {age}',(0,0,255))
        d.text(bottom, emotion,(0,0,255))
      im.save(img)
      return show_picture(img, localfile=True)

    # draw boxes and writes info
    return draw_show_boxes(img, boxes, genders, ages, emotions, localfile)
```

As in `see` and ``read` we use an inner function `draw_show_boxes` to draw the bounding boxes around the detected faces. This is the result:

```
detect_face(source)
```

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*TMIAifocq8z2Hf1kq3sM8w.png)

> Cool right?

These are all the functions we will try regarding images. But let’s experiment with them a little further by taking a picture with our device using the function take_picture (see in the notebook).

### Capture and Send a Picture

Let’s take a picture and see what the cloud thinks about it. Inside all of our functions, we can specify the argument localfile=True to allow us to send local files as binary images.

```python
# turns on the camera and shows button to take a picture
img = take_picture('photo.jpg')
```

Now let’s see what the cloud “thinks” about it by applying the `describe` and `classify` functions:

```bash

print(describe(img, localfile=True, number_of_descriptions=3))

>> A man sitting in a dark room 
>> A man in a dark room 
>> A man standing in a dark roomprint(classify(img, localfile=True))
>> Categories in Image:   dark_fire 
>> Tags found in the image    ['person', 'indoor', 'man', 'dark', 'sitting', 'looking', 'lit', 'laptop', 'front', 'room', 'light', 'standing', 'dog', 'watching', 'computer', 'wearing', 'mirror', 'black', 'living', 'night', 'screen', 'table', 'door', 'holding', 'television', 'red', 'cat', 'phone', 'white']
```
## Text Analytics

> Used to analyze unstructured text for tasks such as sentiment analysis, key phrase extraction and language detection

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*D5gAzwH0k3MozCh9)

<div align='center'>
<p>
<a href="https://miro.medium.com/v2/resize:fit:720/format:webp/0*D5gAzwH0k3MozCh9"> Source </a>
</p>
</div>

- `detect_language` : returns the language detected for each string given.
- 
```python
def detect_language(*documents):
    '''Returns language detected for each string in documents (*args)'''
    
    params = urllib.parse.urlencode({}) 
    headers = {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': subscriptions['TextAnalytics']}
    
    # redefine body for proper text format
    body={"documents": []}
    ID = 0
    for document in documents:
       doc = {
            # assign unique idea
            "id": ID,
            "text": "'{}'".format(document)
        }
       body['documents'].append(doc)
       ID+= 1
    body = json.dumps(body)
    
    r = send_request('/text/analytics/v2.0/languages', params=params, headers=headers, body=body)
    return  [i['detectedLanguages'][0]['name'] for i in  r['documents']]
```

```bash
detect_language('Was soll das?', 'No tengo ni idea', "Don't look at me!", 'ごめんなさい', 'Sacré bleu!')>> ['German', 'Spanish', 'English', 'Japanese', 'French']
```

`key_phrases` : returns a list of the keys (important, relevant textual points) for each given string. If none are found, an empty list is returned.

```python
def key_phrases(*documents):
    # Define the parameters
    params = urllib.parse.urlencode({})
    headers = {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': subscriptions['TextAnalytics']}
    body={"documents": []}
    ID = 0
    for document in documents:
       doc = {
            # assign unique idea
            "id": ID,
            "text": "'{}'".format(document)
        }
       body['documents'].append(doc)
       ID+= 1
    body = json.dumps(body)
    r = send_request('/text/analytics/v2.0/keyPhrases', headers, body, params)
    # _ _ _ _ _ _ _ 
    results = []
    count = 0
    for document in r['documents']:
        doc = []
        for phrase in document['keyPhrases']:
          doc.append(f"{phrase}")
        count += 1
        results.append(doc)
    return results
```

```bash
keys = key_phrases('I just spoke with the supreme leader of the galactic federation', 'I was dismissed', 'But I managed to steal the key', 'It was in his coat')for key in keys:
    print(key)>> ['supreme leader', 'galactic federation'] 
>> [] 
>> ['key'] 
>> ['coat']
```

- `check_sentiment`: assigns a positive, negative or neutral sentiment to the strings given.
  
```python
def check_sentiment(*documents):
     # Define the parameters
    headers = {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': subscriptions['TextAnalytics']}
    params = urllib.parse.urlencode({})
    ID = 0
    body={"documents": []}
    for document in documents:
       doc = {
            # assign unique idea
            "id": ID,
            "text": "'{}'".format(document)
        }
       body['documents'].append(doc)
       ID+=1
    body = json.dumps(body)
    r = send_request('/text/analytics/v2.0/sentiment', headers, body, params)
    # _ _ _ _ _ _ _ 
    sentiments = []
    for document in r['documents']:
      sentiment = "negative"
      # if it's more than 0.5, consider the sentiment to be positive.
      if document["score"] >= 0.5:
        sentiment = "positive"
      elif document['score'] == 0.5:
        sentiment = 'neutral'
      sentiments.append(sentiment)
    return sentiments
```

```bash
print(check_sentiment('Not bad', "Not good", 'Good to know', 'Not bad to know', "I didn't eat the hot dog", 'Kill all the aliens'))>> ['positive', 'negative', 'positive', 'positive', 'negative', 'negative']
```

- `find_entities` : returns a recognized list of entities, assigned to a category. If possible, also a Wikipedia link that refers to the entity.

```python
def find_entities(*documents):  
  """Returns a list of entities recognized in each document of `documents`"""
  headers = {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key': subscriptions['TextAnalytics']}
  params = urllib.parse.urlencode({})
  ID = 0
  body={"documents": []}
  for document in documents:
      doc = {
          "id": ID,
          "text": "'{}'".format(document)
      }
      body['documents'].append(doc)
      ID+=1
  body = json.dumps(body)
  r = send_request('/text/analytics/v2.1/entities', headers, body, params)
  # _ _ _ _ _ _ _ 
  results = []
  for doc in r['documents']:
    names, types, links = [], [], []
    for e in doc['entities']:
      names.append(f"Entity: {e['name']}, ")
      types.append(f"Type: {e['type']}, ")
      try:
        # if wikipedia score above a minimum a treshold append it:
        if e['matches'][0]['wikipediaScore'] >= 0.10:
          links.append(f"Link {e['wikipediaUrl']}")
      except:
        links.append(None)
    docs = []
    for n, t, l in zip(names, types, links):
      if l == None:
        doc_info= n + t
      else:
        doc_info = n + t + l
      docs.append(doc_info)
    results.append(docs)
  return results
```
```bash
find_entities('Lisa attended the lecture of Richard Feynmann at Cornell')

>> [['Entity: Lisa, Type: Person', 
     'Entity: Richard Feynman, Type: Person,
       Link:https://en.wikipedia.org/wiki/Richard_Feynman',   
     'Entity: Cornell University, Type: Organization, 
       Link https://en.wikipedia.org/wiki/Cornell_University']]
```

### OCR + Text Analytics

Let’s see how we can couple some stuff together. Applying read on an image effectively generates textual data for you. So it is the perfect match to apply our text analytics. Let’s make up a report function that extracts the individual text regions, analyzes them and makes up a report with our results:

```python
def report(img, localfile=False):
  """Recognizes and displays regions in the image with text.
  Extracts text and analyzes its language, sentiment, key phrases and entities
  Displays nicely in Markdown"""
  # list of texts in all regions recognized with OCR
  regions = read(img, localfile)
  # list of languages for all regions
  langs = detect_language(*regions)
  # list of sentiments for all regions
  sentiments = check_sentiment(*regions)
  # list of entitites for all regions
  entities = find_entities(*regions)
  # list of key phrases for all regions
  keys = key_phrases(*regions)

  # looping over lists to print results. 
  m = '# Report \n'
  count = 0
  for r, l, s, e, k in zip(regions,langs, sentiments, entities, keys):
    m += f'## Region {count}\n'
    m +='> "'+r[0:82].rstrip()+'...'+'"\n'
    m += f'- Language: {l}\n'
    m += f'- Sentiment: {s}\n'
    m += f'- Entities: {e}\n'
    m += f'- Keys : {k}\n\n'
    count+=1
  return print(m)
```
```python
report(source)
```

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*sf6GpNL6ERqEN2CZBJQx4A.png)

```
# Report  ## Region 1 
> "o. 4230..." 
- Language: English 
- Sentiment: positive 
- Entities:  
    - 4230, Type: Quantity,  
- Keys:  ## Region 2 
> "WASHINGTON, SATURDAY, APRIL 14, 1866..." 
- Language: English 
- Sentiment: positive 
- Entities:  
    - WASHINGTON, Type: Location,   
    - SATURDAY, APRIL 14, 1866, Type: DateTime,   
    - April 14, Type: Other, Wiki [Link](https://en.wikipedia.org/wiki/April_14) 
- Keys:  
    - WASHINGTON ## Region 3 
> "PRICE TEN CENTS..." 
- Language: English 
- Sentiment: positive
- Entities:  
    - Tencent, Type: Organization, Wiki [Link](https://en.wikipedia.org/wiki/Tencent)  
    - TEN CENTS, Type: Quantity,  
- Keys:  
    - PRICE  
    - CENTS...
```

## Speech Services

> To convert audio to text and text-to-speech

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*9ExEDXTfnEyXZpi9)

<div align='center'>
<p>
<a href="https://miro.medium.com/v2/resize:fit:720/format:webp/0*9ExEDXTfnEyXZpi9"> Source </a>
</p>
</div>

We will be using Speech Services to transform voice to text and vice-versa. Together with `record_audio` and `play_audio` (defined in the notebook) we have a way to hear and talk to our notebook. But before we can use Speech Services, we need to get a token (a secret string) that is valid for 10 minutes. We will do this with the function `get_token` as seen below:

### Getting a token

```python
def get_token(subscription_key):
    '''Retrieves a token for Speech Services'''
    global last_time, last_token
    this_time = time.time()
    fetch_token_url = 'https://westeurope.api.cognitive.microsoft.com/sts/v1.0/issueToken'
    headers = {
        'Ocp-Apim-Subscription-Key': subscription_key
    }
    try:
      # if more than 9.5 minutes have passed retrieve and return a new token
      if (this_time - last_time)/60 >= 9.5:
        response = requests.post(fetch_token_url, headers=headers)
        new_token = str(response.text)
        last_time = time.time()
        last_token = new_token
        print('9.5 minutes have passed, a new token has been retrieved')
        return new_token
      else:
        print('9.5 minutes have not passed, using last available token')
    except: # assuming the server works correctly, this can only mean `last_time` does not exist
      # it is the first time that we execute this function.
      last_time = time.time()
      response = requests.post(fetch_token_url, headers=headers)
      last_token = str(response.text)
      # first token ever requested
      print('First token retrieved')
      return last_token
    # last available token
    return last_token
```

We will use it to define the `headers` of our requests inside our functions and authorize them to use our Speech Services resource.

### Speech To Text 

- `speech_to_text` : receives the file path of an audio file and transforms the recognized speech of the given language into text. A whole lot of languages are supported.

```python
def speech_to_text(path_to_wav, language='en-US'):
    '''Input am audio .wav file, returns text with words recognized in the given language
    See docs for valid arguments as language: https://docs.microsoft.com/en-gb/azure/cognitive-services/speech-service/language-support#speech-to-text'''
    # opening audio file
    with open("{}".format(path_to_wav), mode="rb") as audio_file:
        audio_data =  audio_file.read()
    
    speechToTextEndPoint = "https://westeurope.stt.speech.microsoft.com/speech/recognition/conversation/cognitiveservices/v1"
    headers = {"Content-type": "audio/wav; codec=audio/pcm; samplerate=16000", 
            "Authorization": "Bearer " + get_token(subscriptions['SpeechServices']),
            "Expect":"100-continue"}
    params = {"language":language}
    body = audio_data

    # Connect to server, post the request, and get the result
    response = requests.post(speechToTextEndPoint,data=body, params=params, headers=headers)
    result = str(response.text)
    return json.loads(result)['DisplayText']
```

### Text to Speech

- `text_to_speech` : does the exact opposite and transforms the given text into speech (an audio file) with an almost human voice. This will give a voice to our notebook.

```python
def text_to_speech(text):
  headers = {
      "X-Microsoft-OutputFormat": 'riff-24khz-16bit-mono-pcm',
      'Content-Type':'application/ssml+xml',
      'Authorization': "Bearer " + get_token(subscriptions['SpeechServices']),
      "User-Agent": "SmartColabNotebook"
  }
  endpoint = 'https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1'
  xml_body = ElementTree.Element('speak', version='1.0')
  xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
  voice = ElementTree.SubElement(xml_body, 'voice')
  voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
  voice.set('name', 'Microsoft Server Speech Text to Speech Voice (en-US, Guy24KRUS)')
  voice.text = text
  body = ElementTree.tostring(xml_body)

  r = requests.post(endpoint, headers=headers, data=body)
  if r.status_code == 200:
    with open('temp.wav', 'wb') as audio:
      audio.write(r.content)
      print("\nStatus code: " + str(r.status_code) + "\nYour TTS is ready for playback.\n")
      return "temp.wav"
  return "TTS could not be made"
```

Because `speech_to_text` receives an audio file and returns words and `text_to_speech` receives words and returns an audio file, we can do something like this:
```python
# transform words to speech
audio = text_to_speech("Hi, I'm your virtual assistant")# transcribe speech back to words
words = speech_to_text(audio)print(words)
>> Hi I am your virtual assistant
```

Ok cool! But that seems totally pointless. Let’s do something more interesting. We will record our voice with `record_audio`, transform it into words with `speech_to_text` do something with the words and speak out loud the results.

Let’s check up the feeling of what you say with `check_sentiment` :

```bash
# speak into the mic
my_voice = record_audio('audio.wav')# transform it into words 
text = speech_to_text(my_voice)# analyze its feeling
sentiment = check_sentiment(text)[0]# convert analysis into speech
diagnosis = text_to_speech(sentiment)# hear the results
play_audio(diagnosis)
```
Let’s implement this idea inside a function to make it more usable:

```python
def motivational_bot():
  '''Checks for feeling in your speech and says something'''
  voice = record_audio('voice.wav')
  clear_output()
  text = speech_to_text(voice)
  sentiment = check_sentiment(text)[0]
  if sentiment == 'negative':
    voice = text_to_speech("Call 911. You are in deep trouble")
  else:
    voice = text_to_speech("Nice to see you so positive today! Keep it up!")
  return play_audio(voice)
```
Try it out!

```python
motivational_bot()
```

Having your voice converted into voice means you can use your voice as **input into your functions**. There is a really lot you can try out with something like this. For example, instead of checking for feelings in the words you say, you could translate them to a bunch of different languages (see the [Text Translator API](https://azure.microsoft.com/en-us/services/cognitive-services/translator-text-api/) , look up for something on the web (see Bing Search) or (going beyond Azure) maybe ask complex questions to answer engines like [Wolfram Alpha](https://products.wolframalpha.com/simple-api/documentation/) etc.

## LUIS — Language Understanding

> A machine learning-based service to build natural language understanding into apps, bots and IoT devices.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*AM4k3_sMDz471dSn)

<div align='center'>
<p>
<a href="https://miro.medium.com/v2/resize:fit:720/format:webp/0*AM4k3_sMDz471dSn"> Source </a>
</p>
</div>

Let’s make our notebook more intelligent by giving it the ability to understand certain intentions in the language using the LUIS API. In short words, we will train a linguistic model that recognizes certain intentions in the language.

For example, let’s say we have the intent to take_picture. After training our model, if our notebook ‘hears’ sentences of the like of:

- take a photo
- use the camera and take a screenshot
- take a pic

It will know that our intention is to take_picture. We call these phrases utterances. And are what we need to provide to teach the language model how to recognize our intents — the tasks or actions we want to perform.

By using varied and nonredundant utterances, as well as adding additional linguistic components such as entities, roles, and patterns you can create flexible and robust models tailored to your needs. Well implemented language models (backed up by the proper software) are what allow answer engines to respond to questions like “What is the weather in San Francisco?”, “How many kilometers from Warsaw to Prag?”, “How far is the Sun?” etc.

For this post, we will keep things simple and assign 5 utterances to a handful of intents. As you might presume, the intents will match some of the functions that we’ve already implemented.

### Activate LUIS

In contrast to all the services we’ve seen, LUIS is a complex tool that comes with its own “Portal”, where you manage your LUIS apps and create, train, test and iteratively improve your models. But before we can use it, we need to activate a LUIS account. Once you’ve done this:

- Go the LUIS dashboard, retrieve the Authoring Key for your account as seen below and paste it in the notebook.

![](https://miro.medium.com/v2/resize:fit:720/format:webp/1*pI8j1jHH6Lrbfink874Vfw.gif)

```
AuthoringKey = '36bd10b73b144a5ba9cb4126ksdfs82ad2'
```

This was a topic of confusion for me. But the Authoring Key for your LUIS account is not the same as the key for the LUIS resource we made. But you can assign Azure resources to the LUIS app (e.g to open up access points in different regions) but refer here for more detailed information.

### Creating a LUIS app

The LUIS Portal makes it very easy to create, delete and improve your LUIS models. But in this post, we’ll be using the LUIS programmatic API to set things up from within the notebook using the authoring_key.

Let’s start off by creating the app:

```python
def create_luis_app(app_name):
  headers = {
      # Request headers
      'Content-Type': 'application/json',
      'Ocp-Apim-Subscription-Key': authoring_key}

  luis_config = {"name": f"{app_name}",
          "culture": "en-us",
          "description": "Language Understanding for Notebook",
          "InitialVersionId": "1.0",
          }
  body = json.dumps(luis_config)
  params = urllib.parse.urlencode({
  })
  
  r = send_request("/luis/api/v2.0/apps", headers=headers, body=body, params=params)
  return r, luis_config
```

```bash
app_id, luis_config = create_luis_app('Notebot' )
```

In this implementation, we keep track of the app ID (returned by the server) and the parameters that we specified inside app_id and luis_config as global variables for later use.

### dd intents and utterances

Let’s now define a function to add intents and a function to add their respective utterances.

- `create_intent` : adds one intent to the LUIS app. Which app is specified by the variablesapp_id and luis_config.
  
```python
def create_intent(intent):
  headers = {
      # Request headers
      'Content-Type': 'application/json',
      'Ocp-Apim-Subscription-Key': authoring_key,
  }

  params = urllib.parse.urlencode({
  })

  body = {"name": intent}
  body = json.dumps(body)
  try:
      conn = http.client.HTTPSConnection('westeurope.api.cognitive.microsoft.com')
      conn.request("POST", f"/luis/api/v2.0/apps/{app_id}/versions/{luis_config['InitialVersionId']}/intents?%s" % params, body, headers)
      response = conn.getresponse()
      data = response.read()
      print(data)
      conn.close()
  except Exception as e:
      print("[Errno {0}] {1}".format(e.errno, e.strerror))
```

- `add_utterances` : adds a batch of examples/utterances to an existing intent in the LUIS app.
```python
def add_utterances(intent, utterances):
  headers = {
      # Request headers
      'Content-Type': 'application/json',
      'Ocp-Apim-Subscription-Key': authoring_key,
  }

  params = urllib.parse.urlencode({
  })

  body = []
  for utterance in utterances:
    body.append({"text": utterance,
                 "intentName": intent})
  body = json.dumps(body)

  try:
      conn = http.client.HTTPSConnection('westeurope.api.cognitive.microsoft.com')
      conn.request("POST", f"/luis/api/v2.0/apps/{app_id}/versions/{luis_config['InitialVersionId']}/examples?%s" % params, body, headers)
      response = conn.getresponse()
      data = response.read()
      print(data)
      conn.close()
  except Exception as e:
      print("[Errno {0}] {1}".format(e.errno, e.strerror))
```

With these functions, let’s define our language model inside a dictionary as seen below and apply them to it. There is big room for experimentation at this stage.

```python

# a lot of room for experiment here
intentions = {
    
'describe': ["Can you describe what you see",
            "Give a detailed account in words of the picture",
            "Give me some descriptions about the image",
             "Tell me something about the image"],
              
'see': ["What objects can you detect",
        "What objects do you recognize",
        "What things can you percieve"
        "Perform object detection"],

'detect_faces': ["Do you see any humans",
                 "Are there any men in the picture"
                 "Show me the faces",
                 "Do you detect any faces in the picture"], 

"read": ['What can you read',
         "Extract text from a picture",
         "Can you read this out for me please"
         "Read what you are seeing"],
}
```

The keys of this dictionary will be the intents for our application. Let’s loop over them and create them:
```python
intents = intentions.keys()
for intent in intents:
    create_intent(intent)
```
Each intent has 4 examples/utterances, let’s now add these to their respective intents.
```python
for intent, utterances in intentions.items():
    add_utterances(intent=intent, utterances=utterances)
```

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*DOnCP_xeqXQN22LC)

### Train the model

Let’s now train the model with the information we’ve specified with train_luis_app.

```python
def train_luis_app(app_id, luis_config):
  headers = {
      # Request headers
      'Ocp-Apim-Subscription-Key': authoring_key,
  }
  params = urllib.parse.urlencode({
  })
  body = ""
  try:
      conn = http.client.HTTPSConnection('westeurope.api.cognitive.microsoft.com')
      conn.request("POST", f"/luis/api/v2.0/apps/{app_id}/versions/{luis_config['InitialVersionId']}/train?%s" % params, body, headers)
      response = conn.getresponse()
      data = response.read()
      print(data)
      conn.close()
  except Exception as e:
      print("[Errno {0}] {1}".format(e.errno, e.strerror))
```
```
train_luis_app(app_id, luis_config)
```

### Publish the application

We are now ready to publish the application with publish_app.

```python
def publish_app(app_id):
  headers = {
      # Request headers
      'Content-Type': 'application/json',
      'Ocp-Apim-Subscription-Key': authoring_key,
  }

  params = urllib.parse.urlencode({
  })

  body = {
      'versionId': luis_config['InitialVersionId'],
      'isStaging': False,
      'directVersionPublish': False
  }
  body = json.dumps(body)
  try:
      conn = http.client.HTTPSConnection('westeurope.api.cognitive.microsoft.com')
      conn.request("POST", f"/luis/api/v2.0/apps/{app_id}/publish?%s" % params, body, headers)
      response = conn.getresponse()
      data = response.read()
      print(data)
      conn.close()
  except Exception as e:
      print("[Errno {0}] {1}".format(e.errno, e.strerror))
```
```
publish_app(app_id)
```

### Making a prediction

Let’s see if our model is any useful by making predictions of our intents. Note that LUIS has a separate API to make predictions, the LUIS endpoint API.

- `understand`: to predict the intent using the given text

```python
def understand(text, app_id=app_id):
  headers = {
      # Request headers
      'Content-Type': 'application/json',
      'Ocp-Apim-Subscription-Key': authoring_key,
  }

  params = urllib.parse.urlencode({
      # Request parameters
      'appId': app_id})

  body = json.dumps(text)
  try:
      conn = http.client.HTTPSConnection('westeurope.api.cognitive.microsoft.com')
      conn.request("POST", f"/luis/v2.0/apps/{app_id}?%s" % params, body, headers)
      response = conn.getresponse()
      data = response.read()
      data = json.loads(data)
      conn.close()
      try:
        return data['topScoringIntent']['intent']
      except:
        return None
  except Exception as e:
      print("[Errno {0}] {1}".format(e.errno, e.strerror))
```

```python
understand('Can you give me some descriptions about what you are seeing?')# predicted intent is:

>> `describe`understand('Any homo sapiens in the picture?')>> `detect_faces`
```

Cool! Now our notebook can approximately understand what our intentions are from plain language. But having to type the text ourselves doesn’t seem so helpful. The notebook should hear what we say and understand the intention that we have. Let’s address this writing a function hear that uses predict together with the functions `record_audio and `speech_to_text`.

```python
def hear():
  """Hears speech, transforms to text and predicts using the LUIS model specified by app_id
     Returns the predicted intent """
  voice = record_audio("Press ENTER and say a command",'voice.wav')
  clear_output()
  try:
    text = speech_to_text(voice)
  except:
    print('Could not understand')
    return 
  intent = understand(text)
  if intent != None:
    return intent
```

We can now call hear to speak into the mic, transfer our speech into words and predict the intention that we mean using our LUIS app.

```python
intent = hear()# see the prediction
print(intent)
```

### Using the app

Let’s write a function that triggers a set of actions based on the predicted or recognized intent and maps then to execute some code.

In short: a function to execute what happens when a certain intent is predicted. There is **big room** for experiments here.

```python

def Notebot():
  '''Executes a set of actions based on what you say'''
  # greeting message
  speak('Hi there!')
  # sleep while sound is being played
  time.sleep(1)
  clear_output()
  
  def run():
    speak('What can I do for you?')
    time.sleep(2)
    # record voice, transform to text and predict the intention
    intent = hear()
    # carry out the predicted intent
    if intent == 'describe':
      speak("Alright, take a picture and I will describe it for you")
      img = take_picture('photo.jpg')
      clear_output()
      description = describe(img, localfile=True)
      print(description)
      speak('That looks like ' + description.strip())
      time.sleep(3)

    elif intent == 'see':
      speak("Alright, take a picture and I'll see what objects I can find")
      img = take_picture('photo.jpg')
      clear_output()
      # recognize objects 
      objects = see(img, localfile=True)
      # making up results into a sentence
      result = 'I can see '
      for i in range(len(objects)):
        if len(objects) > 1 and i == len(objects) - 1:
          result = result[:-2] + f' and a {objects[i]}.'
        else:
          result += f'a {objects[i]}, '
      # speak out loud the results
      speak(result)
      time.sleep(len(objects))

    elif intent == 'detect_faces':
      speak("Alright, take a picture and I'll find any humans")
      img = take_picture('photo.jpg')
      clear_output()
      detect_face(img, localfile=True)

    elif intent == "read":
      speak("Alright, put a document in front of me and I'll read it for you")
      img = take_picture('photo.jpg')
      clear_output()
      text = read(img, localfile=True)
      result = ' '.join(text).replace('\n', ' ')
      if result == '':
        speak("Hmm I couldn't find any text to read")
        time.sleep(2)
      speak(result)
      time.sleep(len(result.split())/2)

    # if no intent can be recognized, intent will be None
    else:
      speak("Sorry I could not understand you")
      time.sleep(2)
      clear_output()
      run()

  run()
  return speak('I hope that was useful')
```

To finalize, let’s summon the *Notebot* to fulfill our wishes:


![Portal Robot GIF](https://media.giphy.com/media/W0bINkb9yYoYU/giphy.gif)

Depending on what you say, the “Notebot” can take a picture and:

- speak out loud a description
- display any detected objects
- display any detected faces.
- apply OCR and read out loud the results.

```python
# summon your creation
Notebot()
```

The `Notebot` will run a set of actions based on what you say.

Let’s sum up what happens when you call it. In the beginning, you will hear a greeting message. After that the Notebot will apply hear and start recording anything you say, your speech (the percept) will be transcribed to words and sent to the LUIS application to predict the intention that you have. Based on this prediction a different set of actions will be executed. In case there is no clear intent recognized from your speech, the intent “None” will be predicted and the Notebot will call itself again.

Looked from above, the `Notebot` ends up acting as a simple reflex-based agent, that simply finds a rule whose condition matches the current situation and executes it. (In this case what the `Notebot does if you say this or something else`).

At this point, you might like to `upgrade your agent` with additional concepts, e.g adding memory about what is perceived. But I’ll leave that task for the diligent reader.

### Cleaning up

This article got way longer than I intended. Let’s clean up things before we finish. To clean up everything we made in the cloud: delete the entire resource group (along with all the resources) and the `keys.py` file (with your credentials) in the command line >_ by running:

```bash
az group delete --name MyGroup 
rm keys.py
```

Alright, I hope this tutorial gave you at least a couple of ideas to implement in your projects.

> That’s all from me! Thank you for reading the whole thing :)