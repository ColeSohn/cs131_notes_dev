---
title: Detecting Objects by Parts
keywords: (insert comma-separated keywords here)
order: 14 # Lecture number for 2020
---

Object Detection: Task & Evaluation
===================================

Task Definition
---------------

Object detection is the task of detecting or localizing generic objects
from various categories, such as people, animals, objects, etc. More
plainly, with object detection we want to be able to tell *what* types
of objects are in an image and *where* they are.

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/detector.png">
  <div class="figcaption">Object Detection Output</div>
</div>

In the image above, we see the output of an algorithm that has detected
a person, dog, and a chair. It’s important to note that with object
detection we are not interested in identifying specific instances of
object, such as who that person in the image is.

Some challenges to successful object detection can include variations in
illumination and viewpoints, deformations, and intra-class variability.

Benchmarks
----------

Benchmarks are essential to evaluating the progress of an object
detector. One of the earliest benchmarks was the Pascal VOC challenge,
which had a dataset of 20 different categories. The team that compiled
this dataset also organized challenges every year to encourage
researchers to compete in building the best object detection algorithms.
Following the Pascal VOC challenge, the ImageNet Large Scale Visual
Challenge emerged in which researchers competed to build the best object
detector that could detect over 200 categories. One of the more recent
object detection challenges includes the Common Objects in Context
(COCO) challenge, which has only 80 categories but includes detecting
objects with segmentation masks (instead of simple bounding boxes).
Below is an sample of images from the COCO detection challenge and their
segmentations.

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/coco.png">
  <div class="figcaption">Sample images from COCO challenge</div>
</div>

Evaluation
----------

The most common metrics for object detection evaluation include **true
positives** (TP), **false positives** (FP), and **false negatives**
(FN). Below we have defined each:

1.  True Positive: When the overlap with the ground truth prediction is
    greater than 50%.

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/truepos.png">
    </div>

2.  False Positive: When the overlap with the ground truth prediction is
    less than 50%
    
    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/falsepos.png">
    </div>

3.  False Negative: The objects that our model fails to find.

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/falseneg.png">
    </div>

    (Aside: True Negative (TN) refers to areas in the image where our
    algorithm did not produce any prediction because there were no
    ground truth boxes in that region. This metric is not generally
    measured.)

Using the definitions above, we can summarize them in a table. Three
equivalent ways of displaying the combination of predictions and truth
values with distinct terminology are shown below.

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/table1.png">
  <div class="figcaption">Table of predictions and true values using full
terminology</div>
  <img src="{{ site.baseurl }}/assets/images/table2.png">
  <div class="figcaption">Table of predictions and true values using abbreviated
terminology</div>
  <img src="{{ site.baseurl }}/assets/images/table3.png">
  <div class="figcaption">Table of predictions and true values using alternate
terminology</div>
</div>

Utilizing the terminology from Figure 3 above, we can compute
**precision** and **recall**.

$$Precision = \frac{TP}{TP + FP}$$

Precision answers the question: how many of the object detections are
correct?

$$Recall = \frac{TP}{TP + FN}$$

Recall answers the question: how many of the ground truth objects can
the model detect?

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/example calculation.jpg">
  <div class="figcaption">Example Precision and Recall Calculation</div>
</div>

Precision $= \frac{TP}{TP + FP} = \frac{1}{1 + 2} = \frac{1}{3}$

Recall $= \frac{TP}{TP + FN} = \frac{1}{1 + 1} = \frac{1}{2}$

Through varying threshold, we can vary precision and recall.

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/threshold0.png">
  <div class="figcaption">Example with Threshold 0</div>
</div>

When using a threshold of 0, we get a precision of
$ \frac{2}{2 + 5} = \frac{2}{7}$ and a recall of $\frac{2}{2 + 0} = 1.$
We can improve our precision from the by adjusting our threshold to
$0.5$ as shown in the example below.

<div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/threshold0.5.png">
      <div class="figcaption">Example with Threshold of 0.5</div>
</div>

Now, we achieve a precision of $ \frac{1}{1 + 2} = \frac{1}{3}$ and a
recall of $\frac{1}{1 + 1} = \frac{1}{2}$.

**Precision-recall Curve (PR Curve)**

Using different thresholds can lead to different precision and recall
values. Therefore, as shown in the figure below, we can use a PR curve,
a plot of the precision and recall values for every single possible
threshold value which allows us to compare algorithms without worrying
about varying threshold values - here we just take into account all
threshold values instead!

<div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/pcurve.png">
</div>

Siddhartha’s notes (13:00-20:00) {#siddharthas-notes-1300-2000 .unnumbered}
================================

Now we need a metric by which we can measure the performance of
different algorithms and the predictions they make

1.  We use a score to measure the performance of our predictions.

2.  We can then set a threshold which tells our model to only output
    predictions whose score falls within that threshold. This allows our
    model to discard predictions that are perform poorly when it comes
    to precision and recall and only output predictions that meet a
    certain standard.

3.  Of course, it must be noted that by setting a threshold, we are
    changing the recall and precision values because we are discarding
    predictions which would be considered in our calculations. So how do
    we know what threshold to use?

**Comparing Algorithms**
    
<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/Screen Shot 2020-10-28 at 11.53.27 AM.png">
</div>

1.  In the image above we can see that when comparing the PR curves of
    different algorithms, we can use metrics like area under the curve
    as a measure of better performance. A perfect prediction (in pink)
    maximizes the area under the curve.

2.  Or, we might want a certain precision value, and then we would pick
    out the algorithm that has the highest recall for that given
    precision value.

3.  It is important that we use this sort of metric to measure
    performance because different algorithms perform differently at
    varying precision and recall levels - not all of them are shaped the
    way they are in the above example.

**True Positives and False Positives**

1.  Examining what images result on TPs and FPs is useful in improving
    the performance of the model because we can gauge what works for the
    model and what doesn’t.

2.  We also want to look at Near misses, cases in which the localization
    is not precise and the output didn’t have enough overlap with the
    ground truth- for example, if there are many people bunched
    together, our prediction may not be as good.

Rebecca’s notes (20-25) {#rebeccas-notes-20-25 .unnumbered}
=======================

Dalal-Triggs Window
===================

**Key Idea:** Use a sliding window approach to analyze each image
location and decide if there is an object present or not.

This concept is similar to convolution/correlation. The difference is
that we are employing a more sophisticated template (this time we are
basing it on HoG features).

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/hogrecap.png">
  <div class="figcaption">Finding a HoG template for a given image of a person.</div>
</div>

Using the above figure to guide this example, we recall the
visualization of extracting the HoG features of an image. In this
example, we create a person model HoG template (far right) based on the
input image.

Process
-------

1.  Slide window through image and check if there’s an object at every
    location.

2.  Compare HoG feature template to HoG features from each location in
    the image.

3.  If a comparison produces a high score, output detection at the
    corresponding location

Looking at this algorithm, notice that the difference between this and
correlation is that right now, we are comparing HoG features versus
pixel values in the past. Also, an issue that we run into with this
method is that we won’t find the object is we don’t choose our window
size wisely. One solution is to implement the **multiscale sliding
window**: this involves running the method above over lower resolution
of the images. Below is a depiction about the creation of a feature
pyramid.

<div class="fig figcenter fighighlight">
  <img src="{{ site.baseurl }}/assets/images/featurepyramid.png">
  <div class="figcaption">From left to right: resolution pyramid, HoG pyramid, and Filter $F$</div>
</div>

The image above represents the following procedure:

1.  Calculate lower resolutions for our input image.

2.  Create a HoG pyramid from above construction.

3.  Compare filter $F$ to all levels of the HoG pyramid.

4.  Choose location with high score because this entails high chance for
    detection.

Determining the score between a comparison at point $p$ involves the
following equation (displayed in the figure above):
$$F \cdot \phi{(p, H)}$$

where $\phi{(p, H)}$ is equal to the concatenation of HoG features from
subwindow specified by $p$.

Summary
-------

In short, the Dalal-Triggs Window method is basically just computing
similarity between the HoG template and the HoG pyramid. Therefore,
every time we receive a high score, we declare that there is a detected
object.

Peter’s notes (25:00-31:00) {#peters-notes-2500-3100 .unnumbered}
===========================

Deformable Parts Model
======================

Dalal-Triggs Detector Shortcomings
----------------------------------

The Dalal-Triggs Detector is an intuitive model, however, the following
limitations exist in the object filter for the Dala-Triggs Detector:

1.  Global Template. The filter provides no information on fractions or
    pieces of the object, only its entirety. For example, if the filter
    was based upon a person standing, it might not detect that same
    person if their legs were cropped out of the picture.

2.  Rigid Template. Because the HOG features in the filter are static,
    the filter cannot handle pose change or object deformation in
    images. For example, if the filter was based upon a person standing,
    it would not detect that same person running.

Deformable Parts Model
----------------------

1.  The Deformable Parts Model (DPM) is founded upon the a global
    template’s inability to handle fractions or pieces of an object. The
    DPM represents an object as a collection of parts arranged in some
    configuration connected by “springs”. For example, we can link
    various components like the ears and eyes to create something that
    resembles a face, as shown below:

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/deformable1.png">
      <div class="figcaption">Deformable Parts Model of Human Face Connected by Springs</div>
    </div>

2.  Specifically, by capturing specific features of a particular piece
    like the hair and linking it with another piece, the DPM can detect
    not only based off of HOG features, but also with a sense of spatial
    awareness. This also helps with handling pose changes. However,
    connecting the hair with the eyes is a somewhat arbitrary pairwise
    relationship. Instead, we can use a “star model”. The star model
    defines each part relative to a root or anchor. For example, below,
    the blue box around the person serves as an anchor for the spring
    connections with the yellow boxes that represent the person’s head,
    arms, etc.

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/deformable2.png">
      <img src="{{ site.baseurl }}/assets/images/deformable3.png">
      <div class="figcaption">Star Model and Star Model On a Person</div>
    </div>

3.  As such, these models have a global (anchor) filter and a set of
    part filters. To better understand the features of each part, the
    part filters will be at a higher resolution to better capture the
    details. Below is an example of this for a global feature
    representing a person’s body and a part feature of the person’s
    face.

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/deformable4.png">
      <div class="figcaption">Global Filter and an Example of a Part Filter of Human Body and Face</div>
    </div>

    Cole’s notes (31-37) {#coles-notes-31-37 .unnumbered}
    ====================

    Deformations Costs
    ------------------

    As discussed previously, every object has an “anchor” at the center
    of its global filter. We call this its \`\`root position". Likewise,
    every part $P_i$ representing a piece of the full object has an
    anchor at its visual center. Each of these part anchors are likely
    to be a specific offset away from the object’s root position.

    If you are trying to identify a human, for example, it is likely
    that the head will be above the abdomen. As the abdomen is the root
    position for a standing human body, the head’s anchor will have
    little horizontal offset and high vertical offset to the root.

    If the head part filter detects a strong correlation far away from
    the head’s standard anchor position it is likely to be a false
    positive. We should therefore introduce a cost to our model that
    penalizes this deformation.

    We call these cost maps \`\`deformations models" or “allowable part
    locations”. They take the shape of the part filter and can be
    visualized as an elliptical gradient. High color value corresponds
    to high cost. The farther away the part’s center is from the
    expected center, the more cost it will incur.
    
    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/Deformation Model.jpg">
      <div class="figcaption">Deformation Costs of a Standing Human</div>
    </div>

    If we examine the head deformations model in the figure above, we
    see that it takes the shape of a horizontal ellipse. This coincides
    with what we observe to be true in real life. Humans can \`\`deform"
    their head side to side but not much up and down.

    We can also see how different deformation costs have steeper
    penalties. The thighs for example penalize deformations at a steeper
    rate than the head. This also holds true to reality as we expect the
    head position to vary greatly with respect to the abdomen. If thighs
    are detected not directly below the abdomen, however, the object is
    unlikely to be a standing human.\

    It is important to remember that variations in camera position lead
    to perceived variations of objects’ scale. We can account for this
    by convolving object and part features with various resolutions of
    the image to effectively change its scale. We call this a Histogram
    of Gradients Pyramid. It is important to store the level of the
    pyramid when storing root positions of each object and its parts so
    that the final bounding boxes can be drawn to accurate scale and
    relation.
    
    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/Hog Pyramid.jpg">
      <div class="figcaption">HOG Pyramid Example</div>
    </div>
    
    Component Models
    ----------------

    What if, for example, we wanted an object detector that could detect
    humans standing on their heads? Not only would the generic human
    detector shown above and all of its parts be rotated the wrong way,
    but it would also not expect the various changes in arm and hair
    position.

    One easy solution is to have a multiple component human model. This
    means that there would be entirely different root and part filters
    and part filter offsets designed to detect humans that are standing
    on their heads. If using this model returns a high score, the object
    can simply be classified as a human.

    The figure below refers to the more practical example of a two
    component bicycle model. The upper filters and allowable part
    locations depict a side-view while the lower one picture a frontal
    view of a bike. These filters will both classify as \`\`bicycle"
    despite having vastly different appearances.
    
    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/examples/TwoComponentModel.png">
      <div class="figcaption">Two Component Model from Two Views of a Bicycle</div>
    </div>
    
    Neel’s notes (37-43) {#neels-notes-37-43 .unnumbered}
    ====================

    Explanation of Terminology for Each Detection
    ---------------------------------------------

    A model for an object with n parts can be defined as a (n+2) tuple:
    ($F_0$,$P_1$...$P_n, b$), where $F_0$ is the root filter, $P_i$ is
    the ith part model, and b is the bias term.

    Each $P_i$ can then be defined as ($F_i$,$v_i$...$d_i$), where $F_i$
    is the filter for the ith part, $v_i$ is the anchor position for
    part i relative to the root position, and $d_i$ is the deformation
    cost for the placement of the part relative to the anchor position.

    We also need to specify the location of a detection. To do so, we
    have the position z = ($p_0$,...$p_n$) where $p_0$ is the location
    of the root and each $p_i$ is the location of one part in our
    object, and each $p_i$ equal to ($x_i$, $y_i$, $l_i$), where $x_i$
    and $y_i$ are the x and y coordinates of the detection, and $l_i$
    refers to the level of the detection in our pyramid (basically, how
    zoomed in we are).

    Calculating our Scores for each Detection
    -----------------------------------------

    The score for our detection can be defined as the subtraction of two
    sums: The sum of scores for the global and part detectors minus the
    sum of the deformation costs for each part.

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/neel_equation.jpg">
    </div>

    The first term sees how similar the image is to our template, and
    the second term makes sure that the parts are where they’re supposed
    to be, to limit false positives.

    Implementation/Pipeline
    -----------------------

    ​1. Make sure you have filters for the global and the parts $F_i$

    ​2. Compute HOG feature maps from the input image

    ​3. Apply the filters and find the response at each location in the
    image

    Neval’s notes (43-49) {#nevals-notes-43-49 .unnumbered}
    =====================

    ### Accounting for Spatial Cost with a Transformation 

    We want to account for the spatial deformation costs with the
    transformation.

    If we are given the location for the detected head (response map for
    the head), we can guess where the body (root filter) should be. This
    means that when we know that there is a head and we know the
    relative location of the head with respect to the root
    filter($v_i$.), the head location can vote for the possible
    positions of the body (root filter.) So, the body should be in the
    direction calculated from the root person filter: $v_i$.

    However, we allow for some deformation or spatial shift on the
    location of the head with respect to the body: $d_i$. This is due to
    the fact that the head is allowed to deform or be located in
    multiple locations. We account for this by ‘spreading’ the head
    detection when calculating potential locations of the root.

    The result is that we took the head detection map, and we made the
    entire map vote for locations of where the root might be. As a
    result, we apply the deformation to the entire head detection map
    and obtain a new map that is the coordinates of the root.

    Going back to the detection pipeline, we will apply the spatial
    costs for each part:

    We apply the idea to transform the entire response map for all parts
    of the filter to a new space that captures where each part is voting
    for the root filter to be. This means that we can gather information
    from all parts regarding the root filter. All parts vote for where
    the root filter should be, so we can accumulate that with the root
    information which is the response map of root filter. This is simply
    adding the response maps together.

    As a result, we get a combined score of root locations. Finally, we
    use the resulting response map at the end to select the locations of
    where the objects may be in the image.

    Results
    -------

    Deformable Parts Model (DPM) - person

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/person.png">
      <div class="figcaption">DPM - Person</div>
    </div>

    Results – Person detection

    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/person results.png">
    </div>

    Red: best location for the root filter

    Blue: the detector location for the parts

    We allow the model to detect the parts in slightly different
    positions. This is useful for detecting objects in slightly
    different viewpoints or objects that have deformations to the image.

    DPM Summary
    -----------

    Approach:

    This approach is based on manually selecting a set of parts for the
    object and we build a specific detector trained for each part. We
    use a spatial model trained on part activations and evaluate joint
    likelihood of part activations and root filter together.

    Advantages:

    -   Parts have intuitive meaning.

    -   Standard detection approaches can be used for each part of the
        object.

    -   Works well for specific categories.

    Disadvantages:

    -   Parts need to be selected manually.

    -   Semantically motivated parts sometimes don’t have a simple
        appearance distribution.

    -   No guarantee that some important part hasn’t been missed.

    -   When switching to another category, the model has to be rebuilt
        from scratch.

    Extensions - From star shaped model to constellation model
    ----------------------------------------------------------
    
    <div class="fig figcenter fighighlight">
      <img src="{{ site.baseurl }}/assets/images/extension.png">
    </div>

    So far, we have represented the parts and modeled the locations
    relative to a single root part. Which is represented as the “Star”
    shape model. The extension idea that it could be better to have a
    fully connected shape model. This would model the pairwise relation
    (relative location) between every part. This would more strongly
    capture the shape information of the object.


