![Sasquatch Logo](logo.png)
# Sasquatch v0.21
This library contains necessary classes for reading binary sas files. Sasquatch is a AS3 library for reading data sets in the .sas7bdat format, commonly used in the SAS statistical software system.

I have included in the downloads section an installer for a application I built using this library. It is built in Adobe AIR so it should run on all OS that support that.

The majority of the work in this project is derived from Matt Shotwell's sas7bdat project, an R-based SAS reader and the SassyReader project from eobjects.org


## Using the Library
Once you have read a file into a ByteArray? you just need to pass it to the SasFile? object.
```
var sas:SasFile = new SasFile(bytes);
```
or if you need to
```
var sas:SasFile = new SasFile();
sas.loadFromBytes(bytes);
```

the SasFile? will read the binary file and populate two properties for you to use: columns and dataProvider.
now if you had a datagrid named "dg", you can do this:
```
dg.columns = sas.columns;
dg.dataProvider = sas.dataProvider;
```

it is not required to set the datagrid columns flash will figure them out on there own. the columns property is an ArrayCollection? or SasColumns? which extend DataGridColumn?. The purpose for this is so i can extend the functionality of the datagrid column more in order to better present the SasData?.

I will provide proper documentation as soon as possible.

## How to Read a Binary File
```
protected var file:File;
private var stream:FileStream;
        
public function openFile(event:Event):void {
        file = new File();
        file.addEventListener(Event.SELECT, fileOpenSelected);
        file.browseForOpen("Open File");
}
        
/**
 * Called when the user selects the currentFile in the FileOpenPanel control. The method passes 
 * File object pointing to the selected currentFile, and opens a FileStream object in read mode (with a FileMode
 * setting of READ), and modify's the title of the application window based on the filename.
 */
protected function fileOpenSelected(event:Event=null):void { 
        file.removeEventListener(Event.SELECT, fileOpenSelected);
        file = event.target as File;
                
        stream = new FileStream();
        stream.openAsync(file, FileMode.READ);
        stream.addEventListener(Event.COMPLETE, fileReadHandler);
        stream.addEventListener(IOErrorEvent.IO_ERROR, readIOErrorHandler);
}
        
/**
 * Called when the stream object has finished reading the data from the currentFile (in the openFile()
 * method). This method reads the data as UTF data, converts the system-specific line ending characters
 * in the data to the "\n" character, and displays the data in the mainTextField Text component.
 */
private function fileReadHandler(event:Event):void {
        file.removeEventListener(Event.COMPLETE, fileReadHandler);
        
        var bytes:ByteArray = new ByteArray();
        stream.readBytes(bytes);
        stream.close();
                
        var sas:SasFile = new SasFile(bytes);
        // Do something with the SAS file
}
        
/**
 * Handles I/O errors that may come about when opening the currentFile.
 */
private function readIOErrorHandler(event:Event):void {
        trace("The specified currentFile cannot be opened.");
}
```

##Current Features
Load SAS File ( Created with 32-bit version of SAS )
Read SAS Columns: Names, Labels, DataTypes & Formats
Read All Data for Sas file and returns row data.

##Planned Features
--Reading SAS Formats from Catalog File ( sas7bcat )--
Reading SAS Formats from SAS generated Formats XML file
Filtering
Data Visualization
Read files created with 64-bit version of SAS
