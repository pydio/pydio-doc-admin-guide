
<div style="background-color: #fbe9b7;font-size: 14px;">
<span style="background-color: #fae4a6;padding: 10px;">WARNING</span>
<span style="padding: 10px;display: inline-block;">This documentation is for Cells v3. Looking for <a href="https://pydio.com/en/docs/cells/v4/quick-start">Pydio Cells v4 docs?</a></span>
</div>

Image and video annotations are important features that enhance the collaborative aspect of the file sharing platform.  They provides ability to comment out any specific area of an image or a specific timecode inside a video.


## Enabling annotations

By default on the latest release (2.1.0) of Cells, image annotations are enabled but video annotations require manual enabling.

### Image annotations

To use image annotations, open an image with the Cells interface (main application or public link) and hit the annotation menu, a button located on the bottom right.

[:image:4_connecting_your_storage/enabling_annotations/image_annotations_edit.png]

This will open up a menu, you can notice 3 different shapes selectors (**circled by red dotted line on the screenshot below**).

[:image:4_connecting_your_storage/enabling_annotations/image_annotations_menu.png]

You can select any shape (in the screenshot above you can see the 3 different kind), then a cursor will be enabled, click on anything on the image and it will draw the annotation then you can add text to it.


### Video annotations

You need to first enable the annotations, to do so ([enable advanced parameters](./advanced-configurations)) and browse to **Application Parameters >> Available Plugins**.

Search for **video**:

[:image:4_connecting_your_storage/enabling_annotations/video_annotations_plugin.png]

Hit the edit button on the **Video Player** plugin.

[:image:4_connecting_your_storage/enabling_annotations/video_annotations_plugin_enable.png]

then enable **Timecode Annotations** and hit **save**.

Now you can go back and play any video and see the following indications.

[:image:4_connecting_your_storage/enabling_annotations/video_annotations_menu.png]

Hit the display annotation button and then **new**, you can now select any part of the timeline and put a text to indicate what it is.

Once you have added an annotation this will be the end result:

[:image:4_connecting_your_storage/enabling_annotations/video_annotations_example.png]