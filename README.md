# form.lang - FORM syntax highlighting for GtkSourceView

## Description

* `form.lang` is a syntax highlighting definition file that introduces the support for FORM in editors that use GtkSourceView (e.g. GNOME text editor gedit, GNOME Builder etc.) 

* [FORM](https://github.com/vermaseren/form) is a powerful symbolic manipulation system for very large expressions developed by Jos Vermaseren that is widely used in Quantum Field Theory calculations.

## How to install

The instructions on registering FORM files were adapted from <https://help.gnome.org/admin/system-admin-guide/stable/mime-types-custom-user.html.en>

* Download the tarball or clone the repository.
* Create a directory for custom language files in your home directory

```
mkdir -p ~/.local/share/gtksourceview-3.0/language-specs
```

* Copy `form.lang` to `~/.local/share/gtksourceview-3.0/language-specs`

* Create a new mimetype for FORM files in `~/.local/share/mime/packages/text-x-form.xml` with the following content


```
<?xml version="1.0" encoding="UTF-8"?>
<mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
  <mime-type type="text/x-form">
    <comment>new mime type</comment>
    <glob pattern="*.frm"/>
    <glob pattern="*.prc"/>
    <glob pattern="*.h"/>
    <glob pattern="*.hh"/>
  </mime-type>
</mime-info>
```
* Update your mimetypes database
```
update-mime-database ~/.local/share/mime
```
* Create a new .desktop file for FORM in `~/.local/share/applications/form.desktop` with the following content

```
Type=Application
Name=Text Editor
MimeType=text/x-form;
Exec=gedit %f
NoDisplay=true
Icon=gedit
```
* Update your desktop files database

```
update-desktop-database ~/.local/share/applications
```

* Check that FORM files have been registered correctly

```
touch test.frm
gio info test.frm | grep standard
```
The output should be

```
  standard::type: 1
  standard::name: test.frm
  standard::display-name: test.frm
  standard::edit-name: test.frm
  standard::copy-name: test.frm
  standard::icon: text-plain, text-x-generic
  standard::content-type: text/plain
  standard::fast-content-type: text/plain
  standard::size: 0
  standard::allocated-size: 0
  standard::symbolic-icon: text-plain-symbolic, text-x-generic-symbolic, text-plain, text-x-generic
```

* Open any FORM file with gedit and enjoy the new syntax highlighting

## Advanced styling

The files `formext.lang` and `vlad-dark.xml` show how one can customize the highlighting even further by defining a custom style file. The file `formext.lang` should be copied to `~/.local/share/gtksourceview-3.0/language-specs` just like the regular `form.lang` (the two can be installed simultaneously), while the file `vlad-dark.xml` should go into `~/.local/share/gtksourceview-3.0/styles/`

## Screenshots 

Syntax highlighting with `form.lang`

![Alt text](example1.jpg?raw=true)

Syntax highlighting with `formext.lang` and `vlad-dark.xml`

![Alt text](example2.jpg?raw=true)

## Running FORM scripts from gedit

To be able to run your FORM scripts directly from gedit, you need to install and activate the `External Tools` plugin. Then go 
to `Manage External Tools` and create a new tool `Run FORM` with the following script

```
#!/bin/sh
gnome-terminal -- /bin/bash -c 'cd $GEDIT_CURRENT_DOCUMENT_DIR; ~/bin/form -q $GEDIT_CURRENT_DOCUMENT_NAME; echo Hit Enter to close; read'
```

You can also assign it a handy shortcut like Super+F2 to make the execution more convenient.

![Alt text](exttools.png?raw=true)

## Useful links

For more information regarding FORM see

* <https://www.nikhef.nl/~form/>
* <https://github.com/vermaseren/form>
* <https://en.wikipedia.org/wiki/FORM_(symbolic_manipulation_system)>

Here are links to some useful material that I extensively used while creating form.lang:

* <https://developer.gnome.org/gtksourceview/stable/lang-reference.html>
* <https://wiki.gnome.org/Projects/GtkSourceView/StyleSchemes>
* <https://github.com/niklongstone/regular-expression-cheat-sheet>

## License 

`form.lang` and `formext.lang` are covered by the GNU General Public License 3.
