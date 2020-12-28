# What Did It Look Like? (WDILL)

What Did It Look Like? is a service that uses the [Memento framework](http://mementoweb.org/) to poll various public web archives, select an archived version from each calendar year, and create a visualization that shows the progression of the site through out the years. The visualization is shared to the specified [Tumblr](https://www.tumblr.com/) blog, [Instagram](https://www.instagram.com/) page, and [Twitter](https://twitter.com/). In order to nominate a website, tweet `#whatdiditlooklike URL`.

A demo version of this service is deployed and posts to the following accounts:

* Tumblr blog: [whatdiditlooklike.mementoweb.org](https://whatdiditlooklike.mementoweb.org/)
* Twitter: [@_wdill](https://twitter.com/_wdill)
* Instagram: [@_wdill](https://www.instagram.com/_wdill/)

For more explanation of this service, visit:
* [https://ws-dl.blogspot.com/2015/01/2015-02-05-what-did-it-look-like.html](https://ws-dl.blogspot.com/2015/01/2015-02-05-what-did-it-look-like.html)
* [https://ws-dl.blogspot.com/2020/08/2020-08-20-automating-posting-videos-to.html](https://ws-dl.blogspot.com/2020/08/2020-08-20-automating-posting-videos-to.html)

What Did It Look Like is heavily implemented in [Python 3.x](https://docs.python.org/3/) along with some JavaScript. The Python side of the service relies on the following libraries:

* [Requests](https://requests.readthedocs.io/en/master/)
* [Appium Python Client](https://github.com/appium/python-client)
* [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
* [PyTumblr](https://github.com/tumblr/pytumblr)
* [Selenium](https://selenium-python.readthedocs.io/)
* [Surt](https://github.com/rajbot/surt/tree/master/surt)
* [TinyTag](https://github.com/devsnd/tinytag)
* [TLDextract](https://github.com/john-kurkowski/tldextract)
* [Tweepy](https://www.tweepy.org/)
* [Wikipedia](https://github.com/goldsmith/Wikipedia)

The JavaScript dependencies include:
* [Async](https://caolan.github.io/async/v3/)
* [Puppeteer](https://github.com/puppeteer/puppeteer)

## Usage

* Modify the [config](https://github.com/dhruv282/wdill/blob/master/config) file; replace all place holders and insert the necessary credentials.
* The process will run as a cron job on the 1st of every month. To modify the timing, edit the [crontab](https://github.com/dhruv282/wdill/blob/master/crontab) file following [this](https://man7.org/linux/man-pages/man5/crontab.5.html) documentation as needed.


### Running as a Docker Container

Running WDILL in a [Docker](https://www.docker.com/) container can make the process of dependency management easier. This document assumes that you have Docker setup already, if not then follow the [official guide](https://docs.docker.com/installation/).

In order to build the Docker image, clone the repository and change the working directory, make changes to the [config](https://github.com/dhruv282/wdill/blob/master/config) file. Next, build and run the image as the following:

```
$ git clone https://github.com/dhruv282/wdill.git
$ cd wdill
<make necessary changes to config file>
<change cron job timing if needed>
$ docker image build -t wdill .
$ docker run --name wdill --shm-size=1G -it --rm wdill
```

This method will automatically install all required libraries and dependencies and also setup the cron job. The output generated by this service will be stored in `/var/log/cron.log`. Run the following command to view this log:

```
$ docker exec -it wdill cat /var/log/cron.log
```

### Running Locally

Running locally requires the installing programs, libraries, and dependencies manually. This document assumes that you have the following programs installed:

* [Python 3.x](https://docs.python.org/3/) along with [pip](https://pip.pypa.io/en/stable/installing/)
* [Node.js](https://nodejs.org/en/) along with [npm](https://www.npmjs.com/)

Follow the steps below to run this service locally:

```
$ git clone https://github.com/dhruv282/wdill.git
$ cd wdill
<make necessary changes to config file>
<change cron job timing if needed>
$ crontab /etc/cron.d/wdill-cron
$ chmod 0644 /etc/cron.d/wdill-cron
$ touch /var/log/cron.log
$ pip install -r requirements.txt
$ npm install -g
$ cron && tail -f /var/log/cron.log
```