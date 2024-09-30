---
title: "XMLSerializer – what a pain !!"
date: "2006-08-30T15:11:46"
tags: [
  "technical"
]
---
So I’ve spent a lot of time (seems like a huge amount) trying to get to grips with the XMLSerializer in .NET. I think I have pretty much come to the conclusion that what I’m trying to achieve is not possible (even though it seems like a logical thing)…

I’m trying to serialize a bunch of class data into XML format. The actual objects seem to Serialize correctly (if I set the ArrayList, al, to public in the collection) and I can modify the element names in the XML that is produced by using the \[XmlElement(“new\_name”)\] attribute.

However, trying to set the ArrayList to non public and have the XMLSerializer use the C# indexer to get the objects gives me no end of hassle.

1.  If I set the indexer to return objects of the interface type it will always fail
2.  If I change the indexer to return objects of the concrete class then I cannot seem to modify the element name of the concrete class in the XML – it always comes out as ‘MyClass’

I’ve tried overrides, but these also fail telling me I cannot apply a RootAttribute or TypeAttribute to the class

Looks like it’s going to have to be implementing IXMLSerializer instead…

Here’s a snippet of the code that I can’t get working…

public interface IMyInterface

{

    double BigNum { get; set;}

}

public class MyClass : IMyInterface

{

    private double mBigNum = 0;

    public MyClass() { } // default contructor

    \[XmlElement(“bignumber”)\]

    public double BigNum // property

    {

        get { return mBigNum; }

        set { mBigNum = value; }

    }

}

\[XmlRootAttribute(“gpx”)\]

\[XmlInclude(typeof(MyClass))\]

\[XmlInclude(typeof(IMyInterface))\]

public class MyCollection : ICollection

{

    private ArrayList al;

    public MyCollection() { } // default constructor

    public Add(IMyInterface obj) { al.Add(obj); }

    public IMyInterface this\[int index\]

    {

        get { return (IMyInterface)col\[index\]; }

    }

    // ICollection members all implemented correctly

    // IEnumerable members all implememnted correctly

}

// code you want to run to do the serialization

MyCollection mc = new MyCollection

XmlSerializer xSer = new XmlSerializer(typeof(WaypointCollection));

TextWriter writer = new StreamWriter(@”c:myfile.gpx”);

xSer.Serialize(writer, mc);

writer.Close();