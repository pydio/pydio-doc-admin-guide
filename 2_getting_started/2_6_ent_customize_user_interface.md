
_Note: This is an Enterprise Distribution feature_.

Pydio Cells can be easily rebranded: you can customize the User Interface of your instance and have it comply with your enterprise visual identity.

## Display vanitizer interface

To display the main menu of the vanitizer:

- while logged in as an admin user, go to the **Cells Console**,
- toggle the upper right corner switch to display the complete menu in the column on the left.
- open the _Available Plugins_ list by clicking on then to **Application Parameters > Available Plugins**,
- open the **Vanitizer** plugin for edition, configure it to fit your needs (most probably turn both options on):
  - **Display Branding Tool**: show or hide the vanitizer main menu
  - **Use configured branding**: choose between default or customised styling
- save and enable the plugin,
- reload the page (necessary step, do not forget),
- and then go back to your home page.

On the bottom left corner of the page, you now have a menu that looks like the screenshot below:

[:image-popup:2_getting_started/ent_vanitizer.png]

Note that the vanitizer menu stays on top on your interface while you are browsing and editing the look and feel of your app. Once you are done customising, you can save and close the popup by using the buttons that are in its top tool bar.  
To show the branding tool again, you should:

- while logged in as an admin user, go to the **Cells Console**,
- toggle the upper right corner switch to display the complete menu in the column on the left.
- open the _Available Plugins_ list by clicking on then to **Application Parameters > Available Plugins**,
- open the **Vanitizer** plugin for edition
- toggle **ON** the **Display Branding Tool** option and save.
- do not forget to reload the page.

## Branding options

Via the various tabs of the branding tool, you can impact various aspects of your user interface:

- **Palette**: change the colors of the different items such as the menu bar or the buttons.
- **Css**: type raw CSS right here to modify some elements.
- **Buttons**: add new buttons to either trigger a custom javascript or redirect to an external URL.

### Palette

You can use this tab to change the 5 main colors of your interface. This is quite easy to do and modifications are directly visible in your browser.

For the record, we have based the design of the web interface on the [Material Design guidelines](https://material.io). You  might want to have a look at them, and especially on the [page about the palettes and colors](https://material.io/design/color/the-color-system.html) before starting messing around.

### CSS

 We suggest that you to use your browser's _Developer Tool_: you can then directly modify it and monitor the end result in your browser.

### Buttons

This option can be used to add custom button buttons in predefined places of the user interface.
Such buttons can be used to trigger custom javascript or open a user defined link.

It is pretty straight forward:

- name your button
- choose its location
- optionally, add an icon. It is strongly recommended if the other buttons that are in the same menu have icons.
- put your link (with protocol, otherwise the link is considered local by the app) **OR** a Javascript snippet.

Location presets:
 
[:image-popup:2_getting_started/vanitizer/vanitizer_simple_button_layout.png]


#### Example:

| Field Name  | Example setting  | Info about the field
|---|---|---|
| Label  | MyCustomButton  | This field is your button name that will be displayed on the interface |
| Button Location | Left panel widget  | Where the button will be located, in our case it's right next to your username on the left panel |
| Button Icon | [Font Awesome] desktop  | This will display your button as a specific icon (works only for some fields, otherwise the Label is displayed|
| Button Link | empty | In this case this filed is empty |
| Link Target| default setting | Also empty because we are not using the Button Link (this setting works with Button Link)|
| Javascript Snippet| `alert("This will pop an alert on Cells");` | This is where the javascript must be written, here is a simple javascript snippet that will pop an alert when you press the button |

For this example the button will be located here and it will look like this:

- [:image-popup:2_getting_started/vanitizer/button_location_left_panel_widget.png]

### Further customization

You might also have a look at the _Appearance_ page by going to: **Cells Console > Application Parameters > Appearance**.
You can there customize main design artifacts of your Pydio Cell instance, like:

- Background images
- Application title (shown in the end-user's browser tool bar and tabs)
- Logo
- And so on.


### Change the favicon

You can change the Favicon by going to the following menu: **Cells Console > Advanced Settings > UI Customization**.

- Click on the default picture
- A menu will open allowing to put your favicon
- Make sure that your Favicon has the right size
