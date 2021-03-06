# Breaking the Monoform

(Breaking the) Monoform in Public Space” is a work  between a visual artist and a data scientist/developer. It is an open license prototype augmented reality camera application for mobile devices. The camera, with the use of real time image recognition and computer vision techniques, can identify commercial brands in the photographic frame of the user and give them the option to replace them with whatever they intend to. The user can select an image from their own archive and use it to replace the selected advertised space. Computer vision and image recognition is used today on the web, to categorize and auto-brand our user made images. In order to make the users data accessible and available for advertising purposes. Consequently, the user by uploading the image they made on social networks, can not only make a statement about the commercialization of both the virtual and physical public space, but also interfere with the process of advertising in social media. In addition, the user can think of the alternative possible use of the physical public space, re-imagine and create its image  by replacing the space now occupied by commercial advertisements with whatever they want. creating an image might seem immaterial in the physical world, but if we stop and think that this image is taking physical space in a real structure that is used as a storage somewhere and that after its creation its containing data have the potential to be used for profit that will eventually be measured in gold, then the act of changing the image of physical space in its virtual representation starts having a very material shape.

In situ                     |  App view                 |
:----------------------------:|:----------------------------:|
![](assets/4.jpeg)        |![](assets/2.jpeg)    |

In a semantics classifier                     |  How to                  |
:----------------------------:|:----------------------------:|
![](assets/1.jpeg)        |![](assets/3.jpeg)    |

# Monoform?
“Monoform“ is a concept by Peter Watkins that results from his theory about media today and what he calls “the Media Crisis”. Peter Watkins is an English film and television director and one of the pioneers on docudrama. In his book “Media Crisis” he argues, that the role of media today, through popular fiction and documentary film, news media, reality TV, and music video clips, is to create a passive audience. Diminishing the ability of the viewer to reflect and react to the communicated messages of the media.  These messages are often consisted of questionable ethics, promoting stereotypes, nationalism, sexism, and consumerism among others. Monoform is the very fast editing form of the above media, with repetition of images and sounds, short cuts, and a flow of information that gives no time to the viewer to think. It is an attempt to overwhelm the viewer into a passive state of mind. This passivity is then extended to the viewer’s stance towards the society and life itself, thus creating a submissive subject to the state and the current status quo. It’s an idea discussed many times before in the history of art. A similar concept is Guy Debord’s spectacle. In 1967, he made the work “The Society of the Spectacle” in which he conveys that everything is being made in a way to be viewed and not to interact with. The spectacle is the core of non-communication and alienation of people, the opposite of dialogue, creates passivity to the viewer that prevents action towards the viewer’s reality. Albeit from an opposite side of view, Edward Bernays, author of “Propaganda” (1928) expresses the same opinion. He argues that the scientific control of the people is necessary towards the governance of the state and the society. Moreover, he promotes one way of controlling people through controlling their desires. With the use of advertisement, the state can create desires to the people’s minds. These desires become then the driving force for the people to leave the important matters of communal life unattended to the power of the few. The same few are the ones consisting the state but also big private companies collecting eventually the money and the power.

Keeping the above in mind, we argue that the “monoform” exists not only in popular media, but also in the public space, in the center of modern  cities, on our personal screens when Online, inside public transportation etc. Mainly through advertisement, we can see a successful attempt to guide people’s minds towards individualism, creating competitive social environments. A constant race for jobs, money, looks, and safety against others that want the same products and services. 

## How it works
There is a lot of research when it comes object detection from images and for a good reason too: it is a difficult task with many things to consider: Do we use global features such as the distribution of pixel values or local features such as the geometric properties of a subset of the image? Do we want the our recognition to be invariant to shape, scaling and brightness? The list goes on.
 
The problem becomes even harder when we want to make this happen in real time. Naturally, there is not one universal solution that fits all needs. For this particular project we used a method called Haar Cascade classification for three reasons: It is relatively straightforward to understand the concept behind the code, it is well documented and there are ready made tools for the process and it is fast.
 
In a nutshell, we are using a small xml file that contains some of the features of the object we want to detect. A very good explanation of the procedure can be found on the fantastic [OpenCV project website](http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_objdetect/py_face_detection/py_face_detection.html).

The catch is that we have to generate one such file for every single object that we want to detect, and generating such a file is a lengthy procedure and requires a degree of dedication. On the plus side, there are a lot of resources out there that can help you through the process if you want to generate your own cascade: [1](https://pythonprogramming.net/haar-cascade-object-detection-python-opencv-tutorial/), [2](http://docs.opencv.org/2.4.13.2/doc/user_guide/ug_traincascade.html), [3](http://memememememememe.me/post/training-haar-cascades/) to point to just a few. For this project we generated a Haar cascade for the logo of Burger King, but the procedure is similar for any logo out there. This file can be found under res\raw\cascade.xml
 
The Android app itself is composed of four activity classes:
* MainActivity: The introduction window of the app. Tapping the Black Box will navigate to…
* SelectActivity: This class allows one to use one of the preloaded pictures for the logo replacement: a majestic forest or a magnificent blue sky. Tapping on the folder icon you can navigate to the phone's gallery so you can choose any other image you like.
* OpenCVCamera: Tapping the blue sky or forest image launches the object detection class. Here is where all the work is happening.
* OpenCVCameraGallery: Tapping the folder icon of SelectActivity class launches this class. It is essentially the same as OpenCVCamera but now we are fetching and compressing (as far as possible) the image choice of the user.

## What tools
We want to make this work in android phones phones so we need some sorts of android IDE. We are also using the OpenCV library for android, as well as the implementation of the OpenCV for our computers. If you are on mac, homebrew will make your life much easier:

```
brew tap homebrew/science
brew install opencv
``` 

We are using Android opencv library 2.4.11 and not opencv 3 as it was easier to fish out resources for this version online. You can get it from  [here](https://sourceforge.net/projects/opencvlibrary/files/opencv-android/2.4.11/). Also please note that the android code is tested for Android version up to 4.3 (Jelly Bean) so if you try to run the code on devices with API higher than 18 you might get disappointed as we did. The issue was not on the object detection side of things, but rather on the storing and accessing the resulting images which is embarrassing. But then again, we are not Android developers so i'm sure a lot of brilliant people are out there can make it work for more recent versions.

## How to replace a Haar Cascade
OpenCVCamera and OpenCVCameraGallery classes generally do the following:
* Initialise the classifier which translates to loading the Burger King Haar Cascade xml file


``` java
private void initializeOpenCVDependencies() {
      try {
         // Copy the resource into a temp file so OpenCV can load it
          InputStream is = getResources().openRawResource(R.raw.cascade);
          File cascadeDir = getDir("cascade", Context.MODE_PRIVATE);
          File mCascadeFile = new File(cascadeDir, "cascade.xml");
          FileOutputStream os = new FileOutputStream(mCascadeFile);
          byte[] buffer = new byte[4096];
          int bytesRead;
          while ((bytesRead = is.read(buffer)) != -1) {
                  os.write(buffer, 0, bytesRead);
              }
          is.close();
          os.close();
 
          // Load the cascade classifier
          cascadeClassifier = new CascadeClassifier(mCascadeFile.getAbsolutePath());
 
      } catch (Exception e) {
              Log.e("OpenCVActivity", "Error loading cascade", e);
      }
``` 
* Using the classifier to detect the Burger king logo:
``` java
MatOfRect burger_king = new MatOfRect();
 
     // Use the classifier to detect the logo. Play with the parameters to improve detection
      if (cascadeClassifier != null) {
              cascadeClassifier.detectMultiScale(grayscaleImage, burger_king, 1.1,3,3,
                  new Size(absoluteLogoSize, absoluteLogoSize), new Size());
      }

``` 

* Draw a rectangle around that logo and place the image choice
``` java
// Save the logos in an array
Rect[] burger_kingArray = burger_king.toArray();
 
// Placeholder matrix for our image of choice
Mat b = new Mat();
 
// Loop through all logos 
for (int i = 0; i <burger_kingArray.length; i++){
     double xd1 = burger_kingArray[i].tl().x;
     double yd1 = burger_kingArray[i].tl().y;
     double xd2 = burger_kingArray[i].br().x;
     double yd2 = burger_kingArray[i].br().y;
     int ixd1 = (int) xd1;
     int iyd1 = (int) yd1;
     int ixd2 = (int) xd2;
     int iyd2 = (int) yd2;
 
        // Create a rectangle around it
Core.rectangle(inputFrame, burger_kingArray[i].tl(), burger_kingArray[i].br(), new Scalar(0, 0, 0, 0), 0);
     
          Rect roi = new Rect(ixd1, iyd1, ixd2 - ixd1, iyd2 - iyd1);
          
    // Convert the image of choice (bitmap) to the matrix placeholder we defined (b)
          Utils.bitmapToMat(bitmap, b);
          Imgproc.resize(b, b, new Size(ixd2 - ixd1, iyd2 - iyd1));
         
// Place the image of our choice in the rectangle 
          Mat sub  = inputFrame.submat(roi);
          Imgproc.cvtColor(sub, sub, Imgproc.COLOR_RGBA2GRAY);
          Imgproc.cvtColor(sub, sub, Imgproc.COLOR_GRAY2RGB);
          b.copyTo(inputFrame.submat(roi));            
      }
 
    } catch (IOException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
``` 
 

 

