# OCR_HandWritten_Form

In this project, I have filled an SBI Insurance form ([here](https://content.sbigeneral.in/uploads/e1904ff17d084f6582d5cc43bb6e059e.pdf)) by hand, scanned it and performed Optical Character Recognition on the scanned document.

The steps that I took to get the results are contained in the `OCR_Handwritten_Form.ipynb` notebook, and are as follows - 

# Scanning Image and Template
Image refers to the scanned form that I had filled. Template is the image of the original form in the link above. In this section we import these 2 - 
<image src = "Images/scan_2_gray.jpg"></image>
<image src = "Images/template_2_gray.jpg"></image>

# Aligning Images
On Inspecting the 2 images, it was discovered that they were not of the same size. In this section the scanned images are aligned w.r.t the template image. This is done by feature extraction algorithm: **ORB** in OpenCV. In this the key points for the 2 images are generated with ORB, the key points are matched, and the closest keypoints are used to generate a **Warp Perspective** of the image as shown below - 
<image src = "Images/matched_2.jpg"></image>
<image src = "Images/aligned_2.jpg"></image>

# OCR
To extract the handwritten text from the form, several bounding boxes were defined using the template image. These bounding boxes can be found using almost any image editing software (like paint, photoshop, etc). This is why the scanned image was aligned w.r.t the template image. This will ensure that the regions of interest remain consistent for both the scan and the template. 

Each region of interest may contain text or a tick. The tick is identified by taking the sum of the pixels in the centre of the region of interest. The text on the other hand is recognized using a RESNET model. 

The model is trained on the [Kaggle A-Z dataset](https://www.kaggle.com/sachinpatel21/az-handwritten-alphabets-in-csv-format) and the MNIST digits dataset. The model expects the input image to be of shape `(None, 32, 32, 1)`, so in this section the regions of interest are padded and appropriately thresholded to work with the RESNET model. The model was trained using the notebook `Handwritten_Text_Recognition_Model_Training.ipynb`.

The results of the **tick** recognition are - <br>
<image src = "Images/ticks_dict.PNG"></image>

The results of the **text** recognition are - <br>
<image src = "Images/text_dict.PNG"></image>

# Future Scope
- The above results are for the **PROPOSER DETAILS** section on page 1 of the form. The same process needs to be followed for all the other sections of the form.
- Next, a language model needs to be built to make sure that the results are meaningful. For eg, the model must classify **INDIA** text as `INDIA` and not `1ND1A`.
- A more robust bounding box detection algorithm needs to be implemented as this approach required a lot of manual tuning which was not worth the results obtained.

# References
- [[1](https://www.pyimagesearch.com/)] OpenCV and OCR blogs on PyImageSearch
- [[2](https://www.youtube.com/user/Mhproductionhouse)] Youtube Channel: Murtaza's Workshop - Robotics and AI
- [[3](https://www.youtube.com/user/ProgrammingKnowledge)] Youtube Channel: ProgrammingKnowledge 
