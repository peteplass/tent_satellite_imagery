## Finding Tents and Other Emergency Housing Through Exploitation of Satellite Imagery
---
### Problem Statement
During disasters, emergency management agencies and humanitarian organizations need to know where survivors are congregating in order to provide emergency supplies and services. Sometimes, as was the case during the January 2020 earthquakes in Puerto Rico, local communities set up tents as informal shelters. State and national-level organizations may not know about these shelters for days. How can satellite imagery be used to identify informal shelters? Can the same imagery be used to observe changes in the number of tents or occupants? Can we see when a tent shelter is taken down?

---
### Case Study: Haiti Earthquake 2010
On January 12th 2010 Haiti was hit with a catastrophic 7.0 magnitude earthquake, The epicenter was near the town of Léogâne (Ouest), about 16 miles west of the capital, Port-au-Prince. An estimated three million people were affected by the earthquake and its 52 aftershocks of 4.5 magnitude or greater.  The death toll was speculated to range from 100,000 to 316,000. In addition, an  estimated 250,000 residences and 30,000 commercial buildings collapsed or were severely damaged.
Haiti, located on the island of Hispanola in the Caribbean, is in a seismically active region that has a history of destructive earthquakes. In addition, the region is subject to tropical cyclones that frequently cause flooding and widespread damage.

Residents that became homeless from the 2010 earthquake resorted to creating temporary shelters out of tents, tarps and/or other building materials. In Leogane, Haiti, there were sanctioned areas where residents could build tent communities so that they would have quick and easy access to resources such as food, water, and medical care.

We utilized satellite imagery of these tent communities as the basis of our image dataset.

---
### Determining a Method (Part I)
After researching methods for image identification, we came across two main methods: Single Object Image Classification/Localization and Multiple Object Detection/Segmentation. Single Object Classification involves identifying the class that image or single object in an image belongs to (the image is/contains a dog, plane, etc) and Multiple Object Detection involves identifying more than one object in a single image and requires labeling and locating the object in the image.

We came across resources for image identification and successfully completed a tutorial using TensorFlow and Keras in which we used the `keras.datasets` library to load in the `cifar10` dataset and fit a model that accurately classified the image about 50% of the time. This seemed like a great start, but we soon realized that there was no available dataset including `tent` and `not_tent`.

We explored methods of obtaining satellite imagery and researched past projects using satellite imagery that analyzed topics ranging from identifying pools to analyzing population growth in refugee camps. Through these projects, we learned about Hyperspectral Image Classification, specifically, Spectral Image Mapping (SAM). We became interested in exploring how to take on this approach. Because of the hyperspectral nature of satellite imagery, this seemed like the most reasonable procedure.  

What is Hyperspectral Imagery and Spectral Image Mapping?
>"Spectral image mapping is a classic remote sensing technique. The method exploits spectral signature properties to find pixels that have similar spectral signatures, then groups these pixels together. The spectral signature of a pixel contains information on how the material in the pixel reflects or emits light at given points on the EM spectrum." [[source]](https://notebooks.geobigdata.io/hub/tutorials/5c0028260b1ae21bb825284c?tab=code)

---
### Obtaining Data for Hyperspectral Image Analysis
Once we decided to identify tents using hyperspectral image analysis, we explored many ways to obtain satellite imagery. The first method was using the [GBDX](https://www.digitalglobe.com/products/gbdx) satellite imagery platform in combination with their notebook, however we realized that, while GBDX does offer free resources, specifying a location and date for satellite imagery was not going to be free. The second method we approached with was Google Earth. Google Earth allowed us to specify a date by which to pull our images, which was ideal for training our model with pre and post-emergency shelter images for spectral analysis – however, we realized Google Earth images were not hyperspectral and were not high resolution, which are both necessary for spectral analysis. Thirdly, we created an account with Google's Earth Engine API and ran it in Python. While there were many resources on how to use the Google Earth Engine API, they were primarily aimed for JavaScript users. With the Earth Engine API resources we found for Python, we were able to successfully process a map in a Jupyter Notebook, but could not manage to zoom in enough to observe any tents.

After exploring a variety of options, we circled back to GBDX and found an [Open Data Program](https://www.digitalglobe.com/ecosystem/open-data) including high-resolution satellite imagery used for disaster response. These images are stitched renderings of large `.tif` files. The open data included pre and post images of the 2010 earthquake in Haiti. Downloading these images was challenging because of the high amount of images used to stitch together the map region we focused on as well as the size of the file. Once downloaded, preparing the images for spectral analysis was also a challenge. We had to convert the images from `.tif` format to `.lan`, seeing that the Spectral Python (`SPy`) library only works with `.lan`.

Although we converted the image formats, and were able to render a few mappings of spectral bands within an image, due to time constraints and advice from our instructor, we took a different direction for the image analysis.

---
### Determining a Method (Part II) – Object Identification
After realizing that hyperspectral analysis was beyond the scope of the timeline for our project, we considered a more feasible approach by reassessing object detection with a Convolutional Neural Network (`CNN`) Model. Using object detection as our approach would allow us to use the Historical Imagery function in Google Earth (one of our original findings when attempting to utilize spectral angle mapping) to view imagery over Leogane, Haiti in 2010. The satellite images were captured within two weeks of the devastating 7.0 earthquake that struck Haiti. The images contained recordings of emergency tents and housing throughout different locations within Leogane.

---
### Creating Labels for Object Identification
In order to train and test our model, we had to create labels to classify an object in an image as `tent` or `not_tent`. The software [LabelImg](https://github.com/tzutalin/labelImg) allowed us to add a layer of labels to the seven images we obtained from Google Earth. The software was user-friendly and quick to implement. It required us to manually draw label boxes around the objects to identify. The software annotates the labels to its corresponding image by recording four coordinates of the box generated: `xmin`, `xmax`, `ymin`, and `ymax`. We made sure to draw label boxes on multiple `tent` and `not_tent` objects within a single image. The software recorded the box labels as`.xml` files written in `html`.

**Converting `.xml` to `.csv`:**  
With the help of a function from this [tutorial](https://github.com/asetkn/Tutorial-Image-and-Multiple-Bounding-Boxes-Augmentation-for-Deep-Learning-in-4-Steps/blob/master/Tutorial-Image-and-Multiple-Bounding-Boxes-Augmentation-for-Deep-Learning-in-4-Steps.ipynb) and great resource towards informing our labeling and augmenting process, we converted our `.xml` labeled data to a `pandas` readable `.csv` file. Once we had a usable `.csv` file, we could use it to train and evaluate a model.

---
### Modeling

**Validation**

---
### Conclusion
We are extremely close to reaching our goal of creating a fully operational Object Detection Model for tents using Satellite images. When our model is complete, we will be able to deploy it in any scenario in which we would need to locate and count tents in a given region. This is extremely useful in emergency situations, such as after natural disasters or in war torn areas such as Syria.


**Next Steps**  
With a great deal of time and more funding, we could execute the hyperspectral approach by using resources such as GBDX images and pre-trained tent identifying models – or even learn to use ArcGIS. Another technique is to take more time to gather data (most likely cropping images manually) to run through a model in the `TensorFlow` library. With more resources, we would help emergency responders provide emergency supplies and services to those in need more effectively. In addition, our proposed model would help us to collect more accurate real time data about those living without permanent shelter.

---
### Sources
- [LabelImg](https://github.com/tzutalin) – by Darren L Tzutalin
- [Spectral Angle Mapping](https://notebooks.geobigdata.io/hub/tutorials/5c0028260b1ae21bb825284c?tab=code) figures and descriptions
- Haiti 7.0 magnitude [backdrop](https://www.digitalglobe.com/ecosystem/open-data/haiti)
- Flow chart created using [drawio](https://www.draw.io)
- 4 types of deep learning algorithms for object detection [picture](https://medium.com/zylapp/review-of-deep-learning-algorithms-for-object-detection-c1f3d437b852)
- SOS images from Google Earth Haiti 2010 images
- The image of Tents next to the SOS images are from a google earth image over Seattle, WA


*Sources for learning more about hyperspectral imagery:*
- [GISGeography](https://gisgeography.com/multispectral-vs-hyperspectral-imagery-explained/)
- [GBDX Notebook](https://notebooks.geobigdata.io/hub/tutorials/5c0028260b1ae21bb825284c?tab=code)
- [Electromagnetic Spectrum – NASA](https://earthobservatory.nasa.gov/features/RemoteSensing/remote_03.php)
