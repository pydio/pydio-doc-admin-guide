
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

 [TODO] add more details and some examples.

### Buttons

This option can be used to add custom button buttons in predefined places of the user interface.
Such buttons can be used to trigger custom javascript or open a user defined link.

It is pretty straight forward:

- name your button
- choose its location
- optionnaly, add an icon. It is strongly recommanded if the other buttons that are in the same menu have icons.
- put your link (with protocol, otherwise the link is considered local by the app) **OR** a Javascript snippet.

[TODO] give more details about the possible pre-defined location.

### Further customisation

You might also have a look at the _Appearance_ page by going to: **Cells Console > Application Parameters > Appearance**.
You can there customise main design artifacts of your Pydio Cell instance, like:

- Background images
- Application title (shown in the end-user's browser tool bar and tabs)
- Logo
- And so on.
