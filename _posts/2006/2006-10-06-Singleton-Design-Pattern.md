---
title: "Singleton Design Pattern"
date: "2006-10-06T14:50:20"
tags: [
  "development"
]
---
Singleton Design Pattern, is a solution to a common problem in the software domain where you want to create ONE AND ONLY ONE instance of an object/class.

Put simply, it stops client applications creating their own instances of an object and routes all creation of the object through one method. If an instance of the class already exists then that instance is returned, if an instance of the class does not exist then a new instance is created.

So, no matter how many clients access the class there is only ever one instance (that all clients access)

Here’s some sample code of how to achieve a Singleton class :

using System;

using System.Collections.Generic;

using System.Text;

namespace KenAndSarah.GpsControl

{

    public class GpsControl

    {

        private static GpsControl instance = null;

        private GpsControl()

        {

            // do not allow a public constructor

        }

        // call this to get (or create) a reference to the class

        public static GpsControl GetGpsControl()

        {

            if (instance == null)

                instance = new GpsControl();

            return instance;

        }

    }

}

So to clients would simply use it like this

        GpsControl gps = GpsControl.GetGpsControl();

Any subsequent clients who make the same call get a reference to the already created instance of the class.