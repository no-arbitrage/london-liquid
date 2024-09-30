---
title: "MSCRM Customization"
date: "2007-05-25T11:38:31"
tags: [
  "technical"
]
---
One of the projects I am involved with at work was evaluating Microsoft CRM (MSCRM).

Out of the box, it comes as a pretty well fully featured CRM application, but it is also hugely customizable. I downloaded the SDK from [here](http://www.microsoft.com/downloads/details.aspx?familyid=9c178b68-3a06-4898-bc83-bd14b74308c5&displaylang=en) and had a quick play.  
Within 20 – 30 minutes I had a quick extension / customisation working – it is a simple webpage allowing customers to register their own details and when submitted, that customer is automatically added to MSCRM.

It was incredibly simple to get working, just :-

-   Create a new web project
-   Add a web reference to MsCrmSdk
-   Add some textboxes and a submit button and on the ‘submit click’ event just :-
    
    -   Create a new CrmService object
    -   Set the URL for it and provide credentials if necessary
    -   Create a new contact object
    -   Populate it with the data from the textboxes
    -   Call ‘Create’ on the CrmService object passing the contact object as a parameter

You’re done…

Below is some sample code – it only picks up a few details about the contact, but should be fairly easy to enhance … Enjoy

using System;

using System.Data;

using System.Configuration;

using System.Web;

using System.Web.Security;

using System.Web.UI;

using System.Web.UI.WebControls;

using System.Web.UI.WebControls.WebParts;

using System.Web.UI.HtmlControls;

using MsCrmSdk;

    public partial class \_Default : System.Web.UI.Page

    {

        protected void Page\_Load(object sender, EventArgs e)

        {

            btnInsert.Click += new EventHandler(btnInsert\_Click);

        }

        void btnInsert\_Click(object sender, EventArgs e)

        {

            string salutation = txtSalutation.Text;

            string firstName = txtFirstName.Text;

            string lastName = txtLastName.Text;

            string emailAddress = txtEmailAddress.Text;

            CrmService svc = new CrmService();

            svc.Url = “http://10.10.121.226:5555/mscrmservices/2006/crmservice.asmx”;

            svc.Credentials = new System.Net.NetworkCredential(“kenh”, “Exchange1”, “kennet”);

            contact newContact = new contact();

            newContact.salutation = salutation;

            newContact.firstname = firstName;

            newContact.lastname = lastName;

            newContact.emailaddress1 = emailAddress;

            Guid guidResult = svc.Create(newContact);

            txtSalutation.Text = “”;

            txtFirstName.Text = “”;

            txtLastName.Text = guidResult.ToString();

        }

    }