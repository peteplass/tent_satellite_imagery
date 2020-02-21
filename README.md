## Finding Tents and Other Emergency Housing Through Exploitation of Satellite Imagery
---
### Problem Statement
During disasters, emergency management agencies and humanitarian organizations need to know where survivors are congregating in order to provide emergency supplies and services. Sometimes, as was the case during the January 2020 earthquakes in Puerto Rico, local communities set up tents as informal shelters. State and national-level organizations may not know about these shelters for days. How can satellite imagery be used to identify informal shelters? Can the same imagery be used to observe changes in the number of tents or occupants? Can we see when a tent shelter is taken down?

---

### Case Study: Haiti Earthquake 2010


---

### Determining a Method (Part I)
After researching methods for image identification, we came across two main methods: image classification and object detection. Image classification involves determining what class that image belongs to (the image is a dog, plane, etc) and object detection can involve more than one class and requires labeling and locating the object(s) in the image.

We came across resources for image identification and successfully completed a tutorial using TensorFlow and Keras in which we used the `keras.datasets` library to load in the `cifar10` dataset and fit a model that accurately classified the image 75% of the time. This seemed like a great start, but we soon realized that there was no available dataset including `tent` and `not_tent`.

We explored methods of obtaining satellite imagery and researched past projects using satellite imagery that analyzed topics ranging from identifying pools to analyzing population growth in refugee camps. Through these projects, we learned about Hyperspectral Image Classification specifically, Spectral Image Mapping (SAM) method, and became interested in exploring how to take on this approach. Because of the hyperspectral nature of satellite imagery, this seemed like the most reasonable procedure.  

What is Hyperspectral Imagery and Spectral Image Mapping?
>"Spectral image mapping is a classic remote sensing technique. The method exploits spectral signature properties to find pixels that have similar spectral signatures, then groups these pixels together. The spectral signature of a pixel contains information on how the material in the pixel reflects or emits light at given points on the EM spectrum." [[source]](https://notebooks.geobigdata.io/hub/tutorials/5c0028260b1ae21bb825284c?tab=code)

---

### Obtaining Data for Hyperspectral Image Analysis
Once we decided to identify tents using hyperspectral image analysis, we explored many ways to obtain satellite imagery. The first method was using the [GBDX](https://www.digitalglobe.com/products/gbdx) satellite imagery platform in combination with their notebook, however we realized that, while GBDX does offer free resources, specifying a location and date for satellite imagery was not going to be free. The second method we approached was using Google Earth. Google Earth allowed us to specify a date by which to pull our images, which was ideal for training our model with pre and post-emergency shelter images for spectral analysis – however, we realized Google Earth images were not spectral and were not high resolution, which are both necessary for spectral analysis. Thirdly, we created an account with Google's Earth Engine API and ran it in Python. While there were many resources on how to use the Google Earth Engine API, they were primarily aimed for JavaScript users. With the Earth Engine API resources we found for Python, we were able to successfully process a map in a Jupyter Notebook, but could not manage to zoom in enough to observe any tents.

After exploring a variety of options, we circled back to GBDX and found an [Open Data Program](https://www.digitalglobe.com/ecosystem/open-data) including high-resolution satellite imagery used for disaster response. These images are stitched renderings of large `.tif` files. The open data included pre and post images of the 2010 earthquake in Haiti. Downloading these images was challenging because of the high amount of images used to stitch together the map region we focused on as well as the size of the file. Once downloaded, preparing the images for spectral analysis was also a challenge. We had to convert the images from `.tif` form to `.lan` seeing as how the Spectral Python (`SPy`) library required that type of image.

Although we converted the image formats, and were able to render a few mappings of spectral bands within an image, due to time constrictions and advice from our instructor, we took a different direction for the image analysis.

---

### Determining a Method (Part II) – Object Identification
After realizing that hyperspectral analysis was beyond the scope of the timeline for our project, we considered a more feasible approach by reassessing object detection with a Convolutional Neural Network (`CNN`) Model. Using object detection as our approach would allow us to use the Historical Imagery function in Google Earth (one of our original findings for spectral analysis) to view imagery over Leogane, Haiti in 2010. The satellite images were captured within two weeks of the devastating 7.0 earthquake that struck Haiti. The images contained recordings of emergency tents and housing throughout different locations within Leogane.

---

### Creating Labels for Object Identification
In order to train and test our model, we had to create labels to classify an object in an image as `tent` or `not_tent`. The software [LabelImg](https://github.com/tzutalin/labelImg) allowed us to add a layer of labels to the seven images we obtained from Google Earth. The software was user-friendly and quick to implement. It required us to manually draw label boxes around the objects to identify. The software annotates the labels to its corresponding image by recording four coordinates of the box generated: `xmin`, `xmax`, `ymin`, and `ymax`. We made sure to draw label boxes on multiple `tent` and `not_tent` objects within a single image. The software recorded the box labels as`.xml` files written in `html`.

**Converting `.xml` to `.csv`:**  
With a function from this [tutorial](https://github.com/asetkn/Tutorial-Image-and-Multiple-Bounding-Boxes-Augmentation-for-Deep-Learning-in-4-Steps/blob/master/Tutorial-Image-and-Multiple-Bounding-Boxes-Augmentation-for-Deep-Learning-in-4-Steps.ipynb), we converted our `.xml` labeled data to a `pandas` readable `.csv` file.

---
### Modeling

**Validation**

---
### Limitations and Next Steps

---
### Sources
-
-


*Sources for learning more about hyperspectral imagery:*
- [GISGeography](https://gisgeography.com/multispectral-vs-hyperspectral-imagery-explained/)
- [GBDX Notebook](https://notebooks.geobigdata.io/hub/tutorials/5c0028260b1ae21bb825284c?tab=code)
- [Electromagnetic Spectrum – NASA](https://earthobservatory.nasa.gov/features/RemoteSensing/remote_03.php)
