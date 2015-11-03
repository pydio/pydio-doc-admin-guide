### Editors
One of AjaXplorer nice feature is the ability to **preview online** and even **edit** many file extensions, without having to download them to the user desktop. Many editors comes bundled and activated by default, but some more advances features must be activated after installation, as they require some dependencies that generally are not present by default on all servers.

#### _Previewing PDF documents_

To generate thumbnails and web-viewable previews of PDFs document, you have to install Image Magick on your server. This is quite easy (use aptitude/yum for linux-based servers, or download and install for Windows) to do, and you then have to locate the “convert” utilitary. On linux, this is generally in **/usr/bin/convert**. On Windows, make sure to enter a “no-space” path, using the windows shortcuts like **C:\PROGRA~1\IMAGICK\bin\convert.exe** .

Enable this editor by going to Settings panel, under Global Configurations > Plugins > Editor,  Image Magick. Enter the “convert” path, and tweak the default options values if necessary. Leave unoconv disabled for now. Save and close, then switch to a file workspace containing PDFs : you should now see previews of these files, and also for the following formats : **pdf, svg, tif, tiff, psd, xcf, eps, cr2.**

#### _Previewing Office documents_

An additional tool working along with IMagick can enable support for previewing Office documents (xls, xlsx, ods, doc, docx, odt, ppt, pptx, odp, rtf) online.

+ First enable the Image Magick editor as described in the previous section.
+ Install the unoconv utilitary tool on the server. This is a PY based utilitary that makes use of Open Office “headless” binaries ( = command line tool) to convert the office documents to PDF. Thus, OOffice headless must be installed as well.
	- Debian : apt-get install unoconv openoffice.org-headless openoffice.org-writer openoffice.org-calc openoffice.org-impress
	- CentOS : yum install unoconv openoffice.org-headless openoffice.org-writer openoffice.org-calc openoffice.org-impress
+ In the Editor.imagick settings, set “Unoconv Path” to the path to the unoconv binary on the server.

To take the most out of this setup, install and activate the PDF2TEXT utilitary inside the index.lucene plugin. That way, the content of many file types (pdf-likes and office-likes) will be indexed and searchable.

#### _Editing Office documents_

AjaXplorer does not provide an web-based editor for Office documents: technical step for such a tool is quite high! Thus, for this kind of feature, we provide interfaces with an online service, **Zoho**. More services should be provided in the future.

To activate the Zoho viewer, you will first have to sign in to the Zoho service, choose a plan, and get an API Key from your Zoho account. Then, activate and configure the editor.zoho plugin (“Office Docs”) by entering your API Key.

If your server is directly accessible from the web, you should not have to change the “Z-Agent” setup. If your server is behind a firewall or LAN only, you will have to “install a Z-Agent” to provide a bridge between the outside world (= zoho servers) and your AjaXplorer system. This is necessary for Zoho to post back the changes after you have edited a file. The Z-Agent is simply the script contained in the plugins/editor.zoho/agent folder, that you must place on a world-accessible PHP server, and that will be the target of Zoho postback. It will temporary store the new version of the files, and after edition, AjaXplorer will query the agent to get the new version of this file.

### Uploaders
Upload is one of the most used operation in AjaXplorer, and can be a great source of annoyments for the sys admin, depending on how far you can customize your server settings. PHP settings are the first source of limitations, but http requests body length can be limited as well, memory and script time limits, etc.

By default, AjaXplorer is provided with an “auto-switching” uploader :

+ HTML5 Upload : uses latest HTML5 & JS features to enable users multiple selection at once, as well as upload progress bar. Also enables drag&drop feature from the client desktop directly to the browser.
+ If not available (Internet Explorer for example), switches to a FLASH based uploader (more precisely FLEX-based), to provide the same set of features to the users, except drag’n’drop.
+ These two does not support “folder upload”.

Sometimes if your users always upload huge files, or you just can’t raise at all your PHP upload limits, you may want to switch to the Java-based uploader, uploader.jumploader. It’s a Java Applet that will allow additional features : folders at once, and files chunking (uploading files into small pieces to always stay under the upload limit). This is quite handy, but the drawback is that the users must have Java & the java plugin installed on their machine.

To install Jumploader, please follow the plugin instruction : you will first have to actually DOWNLOAD jumploader from the developers website. Same for PLUpload plugin : first download the sources from the developers website.