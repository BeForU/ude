Ude is a C# port of [Mozilla Universal Charset Detector](http://mxr.mozilla.org/mozilla/source/extensions/universalchardet/src/).

The article "[A composite approach to language/encoding detection](http://www.mozilla.org/projects/intl/UniversalCharsetDetection.html)" describes the charsets detection algorithms implemented by the library.

Ude can recognize the following charsets:

* UTF-8
* UTF-16 (BE and LE)
* UTF-32 (BE and LE)
* windows-1252 (mostly equivalent to iso8859-1)
* windows-1251 and ISO-8859-5 (cyrillic)
* windows-1253 and ISO-8859-7 (greek)
* windows-1255 (logical hebrew. Includes ISO-8859-8-I and most of x-mac-hebrew)
* ISO-8859-8 (visual hebrew)
* Big-5
* gb18030 (superset of gb2312)
* HZ-GB-2312
* Shift-JIS
* EUC-KR, EUC-JP, EUC-TW
* ISO-2022-JP, ISO-2022-KR, ISO-2022-CN
* KOI8-R
* x-mac-cyrillic
* IBM855 and IBM866
* X-ISO-10646-UCS-4-3412 and X-ISO-10646-UCS-4-2413 (unusual BOM)
* ASCII

## Platform
Windows and Linux, Unity Engine (Mono)

## Install
The release consists in the main library (*Ude.dll*) and a command-line client (*udetect.exe*) that can be used for one-shot tests.

On Windows, compile the Visual Studio 2005 solution ude.sln. On Linux you can build the library, the example and the nunit tests with monodelop and its solution ude.mds, or using make. To compile the sources tarball:

    $ ./configure.sh --prefix=/usr/local --enable-tests=yes
    $ make
    
To compile from svn:

   $ ./autogen.sh --prefix=/usr/local --enable-tests=yes
   $ make
   
You can pick the library (*Ude.dll*) from the toplevel build directory (*./bin*) or you can install it to *$prefix/lib/ude* by typing:

   $ make install
   
This will installs a command-line example program (*$prefix/bin/udetect*) to test the library on a given file as:

   $ udetect filename 
To run the nunit tests type:

    $ make test
### Unity Engine
Build the project and pick Ude.dll into Assets\Plugins folder.

## Usage
### Example
    public static void Main(String[] args)
    {
        string filename = args[0];
        using (FileStream fs = File.OpenRead(filename)) {
            Ude.CharsetDetector cdet = new Ude.CharsetDetector();
            cdet.Feed(fs);
            cdet.DataEnd();
            if (cdet.Charset != null) {
                Console.WriteLine("Charset: {0}, CodePage: {1}, confidence: {2}", 
                     cdet.Charset, cdet.CodePage, cdet.Confidence);
            } else {
                Console.WriteLine("Detection failed.");
            }
        }
    }    
#### Unity Engine
    public void Start()
    {
        string filename = "sample.txt";
        System.Text.Encoding enc = System.Text.Encoding.Default;
        
        using (FileStream fs = File.OpenRead(filename)) {
            Ude.CharsetDetector cdet = new Ude.CharsetDetector();
            cdet.Feed(fs);
            cdet.DataEnd();
            if (cdet.Charset != null) {
                    System.Text.StringBuilder sb = new System.Text.StringBuilder();
                    sb.Append("Charset: ");
                    sb.Append(cdet.Charset);
                    sb.Append(", CodePage: ");
                    sb.Append(cdet.CodePage);
                    sb.Append(", Confidence: ");
                    sb.Append(cdet.Confidence);

                    Debug.Log(sb.ToString());
                    enc = System.Text.Encoding.GetEncoding(cdet.CodePage);
            } else {
                Console.WriteLine("Detection failed.");
            }
        }
    }  

## Other portings
The original Mozilla Universal Charset Detector has been ported to a variety of languages. Among these, a Java port:

* [juniversalchardet](http://code.google.com/p/juniversalchardet/)

from which I copied a few data structures, and a Python port:

* chardet

## License

The library is subject to the Mozilla Public License Version 1.1 (the "License").