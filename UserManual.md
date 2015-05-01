This is page is the user manual of the crawler

# User Manual #
  * General Information
    * Introduction
> > > ITunes Store is very fashion these days. People can buy the music, game, and book or other things in the store. This picture shows a part of games in the store:
> > > ![http://www.ppseek.com/shareimg/1572074.jpg](http://www.ppseek.com/shareimg/1572074.jpg)<br />
> > > As we can see, there are several attributes of these games just like _Name_, _Artist_, _Price_ and _Popularity_. What the crawler needs to do is to get useful information so that we can analysis which game or music is the most popular one and how will this situation change next week. The examples below are created in Windows, but our project can work in Linux too.

  * Tools
    * WGET
> > > A tool used to send request and get response from the store
    * Fiddler
> > > A tool used to analysis the request we send to ITunes Store and the response we get from the store
  * Getting Started
    * Use Fiddler to get the effective request
      1. If you are not familiar with Fiddler, you can visit this page: http://www.fiddler2.com/fiddler2/ and download it. You can download ITunes from http://www.apple.com/downloads/.<br />
      1. Open Fiddler and ITunes. Choose the thing you are interested in, for example, I choose the _“APP Store”-->”Games”-->”ALL”_:<br />
> > > ![http://www.ppseek.com/shareimg/1572075.jpg](http://www.ppseek.com/shareimg/1572075.jpg)<br />
> > > Fiddler will record the requests and responses like this:<br />
> > > ![http://www.ppseek.com/shareimg/1572076.jpg](http://www.ppseek.com/shareimg/1572076.jpg)<br />
    * Run the project
> > > Now we have got the file we need. We just need to run the project, and then we can check the data in our database.
> > > Pay attention to the attribute _“Body”_, it shows the file’s length. Choose the biggest one and check its information in the Inspectors:<br />
> > > ![http://www.ppseek.com/shareimg/1572077.jpg](http://www.ppseek.com/shareimg/1572077.jpg)<br />
> > > Through this way, we can know the request’s specification including _URL_ and _User-Agent_. Then we will go to the next step.
    * Use WGET to send the request
      1. Download WGET from http://users.ugent.be/~bpuype/wget/. Move the _wget.exe_ file to the directory which you want to save the xml file download from ITunes Store, for example: _E:\workspace\Store File\_.<br />
      1. Open the _cmd.exe_, try this command **_”wget www.google.com ”_** to check if the WGET works well. If you can find _“index.html”_ in the directory _Store File_, WGET is ok now. (Using _“wget -h”_ to show the help).<br />
      1. Now, we will send the request to ITunes Store based on the analysis in Fiddler. Input this command **_“wget –U=iTunes/9.0.3 –O test.xml http://ax.itunes.apple.com/WebObjects/MZStore.woa/wa/browse?path=%2F36%2F6014%2F1”_**and you will get the xml file named _“test”_. The **_“iTunes/9.0.3”_** is User-Agent we got from Fiddler. **_“-O (not zero)”_** is a command used to name the file you download. The address **_“http://ax.itunes.apple.com/WebObjects/MZStore.woa/wa/browse?path=%2F36%2F6014%2F1”_**comes from the part highlight in Fiddler.
    * Create Task Schedule
> > > If we always download the file by ourselves, it seems not very convenient. Task Schedule is a good choice. <br />
      1. We need to change the default path of the cmd. In our case, because we create the director _E:\workspace\Store File\_, so the default path should be _E:\workspace\Store File\_. The method I use to change the default path is:  _Click Start, Run and type regedit_. Follow these steps:_HKEY\_CURRENT\_USER \ Software \ Microsoft \ Command Processor. In the right-pane, create a new value called "Autorun", and then set it's startup folder path, preceded by "CD /D "_. In our case, we should input **_"CD /D E:\workspace\Store File\"_**. Ok, let's open the cmd again, the default path has been changed.<br />
      1. Write the command **_“wget –U=iTunes/9.0.3 –O test.xml http://ax.itunes.apple.com/WebObjects/MZStore.woa/wa/browse?path=/36/6014/1”_**into a txt file, and then save the file as _.bat_file like _“wgetStore.bat”_.      Pay attention to the URL address, have you found the difference between this one and the URL we used before? Yes, the URL we used in cmd was http://ax.itunes.apple.com/WebObjects/MZStore.woa/wa/browse?path=%2F36%2F6014%2F1, but the character **"%"** can't be executed, so we replace it by **"/"**. The information about some special characters can be found in this page: http://en.wikipedia.org/wiki/Percent-encoding.<br />
      1. Now we have prepared all the things well. It's time to create a task schedule. Open the _"Control Panel", "Scheduled Tasks", "Add a new Task"_, now you should choose the bat file we create before, and then set the time you want. Finally, input the password and if there is no password, just click next step, finish. <br />
> > > Maybe someone will see the warning about the password, don't worry, follow the methods list below:<br />
> > > The first situation,  the _"Task Scheduler"_service doesn't start. You can click _Start, Run and type "services.msc"_to check if it is enabled, if no, just start it.<br />
> > > The second situation, your current account was set as logging automatically, but the password is blank, this will lead to the "Task Schedule" can't work well. You need to click _Start, Run, type "gpedit.msc"_and then expand_"Windows settings"->"Security settings""Local Ploicies""Security Options"_. Double click _"Accounts: Limit local account use of blank passwords to console logon only"_ and set it as disabled.<br />
> > > Ok, we have finished the task schedule.
  * Javadoc

> > http://forum-crawler.googlecode.com/svn/trunk/doc/index.html