# Yorkshire Sky Colours @sky_yorkshire

## This is a bot that takes pictures of the sky and analyses the colours it finds. Then it tweets a small square of the average colour of the sky at that time, along with a name for that colour.

The bot runs on a [Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/), using a [standard camera module](https://www.raspberrypi.org/products/camera-module-v2/) to grab the images.

The process is this:

1. Grab an image from the camera and [save it locally](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/5)
2. Using Pillow, load the image and [convert pixels into individual data of Red,Green,Blue values.](https://pillow.readthedocs.io/en/3.0.x/reference/Image.html#PIL.Image.Image.getdata)
3. Select just the pixels that represent the sky. This is done in a dumb way, since the camera is not mobile. I literally entered values to pick out a large rectangle of the sky. The camera has a good view of the sky, so a big sample is taken.
4. Calculate the average Reg, Green and Blue values of these pixels.
5. Again, using Pillow, [generate a small image of this colour](https://pillow.readthedocs.io/en/3.0.x/reference/Image.html#PIL.Image.new) and save it locally.
6. Using the Red, Green, Blue values, [send these over](https://www.w3schools.com/python/module_requests.asp) to an [amazing color-naming API](https://github.com/meodai/color-names), and get the response. [Read the name from the JSON it sends back.](https://developer.rhino3d.com/guides/rhinopython/python-xml-json/)
7. [Connect to Twitter using their developer API and Twython](https://www.instructables.com/id/Raspberry-Pi-Twitterbot/)
8. [Upload the small colour image we made before.](https://twython.readthedocs.io/en/latest/usage/advanced_usage.html#updating-status-with-image)
9. [Read in a text file](https://www.w3schools.com/python/ref_file_readlines.asp) containing a load of prewritten tweets that have a marker where the colour name should be.
10. [Replace](https://www.geeksforgeeks.org/python-string-replace/) the marker with the colour name generated earlier.
11. Post the tweet with the new message.


This is all set up to run on a linux [cron job](https://opensource.com/article/17/11/how-use-cron-linux), triggering once an hour.


### Future develoments

At the moment colour data and images are discarded every time. The next iteration will store colour data in a database and generate a band of colour representing the changing colours throughout the day, then post this at midnight.

At the moment, the code is disgusting. Once it is presentable I'll post it so you can easily reproduce it, or adapt it with your own ideas.
