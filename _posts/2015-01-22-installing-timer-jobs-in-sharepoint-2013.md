---
layout: post
title: Installing custom Timer Jobs in SharePoint 2013
---

{{ page.title }}
================

Status: Draft
-------------

Abstract
--------
There are plenty of articles which more or less detail the title of this post, [msdn for example](https://msdn.microsoft.com/en-us/library/office/hh528519(v=office.14).aspx), this is just what made sense for myself in the context of a SharePoint 2013 Timer Job that requires Web Application Feature scope.

Extend SPJobDefinition
----------------------
First thing you need to do is create a new class that extends SPJobDefinition.
Then create your three constructors and override execute method.

{% highlight c# %}
namespace somenamespace
{

  public class MyTimerJobExecution : SPJobDefinition
  {
    public MyTimerJobExecution() { }

    public MyTimerJobExecution(string jobName, SPService service)
    : base(jobName, service, null, SPJobLockType.None)
    {
      this.Title = jobName;
    }

    public MyTimerJobExecution(string jobName, SPWebApplication webapp)
    : base(jobName, webapp, null, SPJobLockType.ContentDatabase)
    {
      this.Title = jobName;
    }

    public override void Execute(Guid contentDbId)
    {
      try
      {
        // execute your logic here
      }
      catch (Exception ex)
      {
        // do your logging here
        throw;
      }
    }
  }
}
{% endhighlight %}

In this case we will be creating a feature with Web Application scope, which will allow us in a round about way to retrieve the web context derived from the web application context.


Create the Event Receiver
-------------------------
Right click on your feature and select the option to create an event receiver class.
{insert code}


Mind the Manifest
-----------------

{insert code}

Installing
----------

{insert powershell code }


Debugging with ULS
------------------
During your feature install it is advisable to be watching your [ULS](https://msdn.microsoft.com/en-us/library/office/ff512738(v=office.14).aspx) logs in realtime, this will help identify the source of any exceptions. If you exception is complaining about your project assembly missing then it is likely you have an incorrect assembly mapping in you feature's manifest (see above).

Once your Timer Job feature is successfully installed you also want to watch for exceptions from the execution of the Timer Job. So launch your job from with SharePoint Central Admin {insert path} and pretty soon you should see your job kick off under the OWS{fullname} process, any exception raised by your code executed from with the Excecute override of SPJobDefinition method (see above) will now be apparent. As an alternative to ULC logging you can of course attach the Visual Studio debugger to the OWS{fullname} process, this however for reason's unknown to me does not always work in my local development environment.
