Django Youtube
==============

Django Youtube is a wrapper django app around youtube api. It helps you to implement the frequent api operation easily.

The main functionality is to use Youtube API to upload uplisted videos and show them in the website as a social web site do.
Basically implementing video features on a website using Youtube. In order to achieve this goal, you need a developer account on Youtube and use them to authenticate, and upload videos into this account.

Please feel free to contribute and send patches.

Features
--------

1. Retrieve specific videos
2. Retrieve feed by a user
3. Browser based upload
4. Authentication to reach private data
5. Admin panel ready
6. Supports i18n

Features are not yet implemented
--------------------------------

1. Retrieve feeds (most visited etc)
2. Direct upload
3. oAuth authentication

Dependencies
------------

gdata python library

Installation
------------

1. Add 'django_youtube' to your installed apps
2. Add following lines to your settings.py and edit them accordingly
    
    YOUTUBE_AUTH_EMAIL = 'yourmail@gmail.com'
    YOUTUBE_AUTH_PASSWORD = 'yourpassword'
    YOUTUBE_DEVELOPER_KEY = 'developer key, get one from http://code.google.com/apis/youtube/dashboard/'
    YOUTUBE_CLIENT_ID = 'client-id'
    
3. Optionally you can add following lines to your settings. If you don't set them, default settings will be used.

    # url to redirect after upload finishes, default is respected 'video' page
    YOUTUBE_UPLOAD_REDIRECT_URL = '/youtube/videos/'
    
    # url to redirect after deletion video, default is 'upload page'
    YOUTUBE_DELETE_REDIRECT_URL = '/myurl/'
    
3. Add Following lines to your urls.py file
    
    (r'^youtube/', include('django_youtube.urls')),
    
4. Don't forget to run 'manage.py syncdb'

Usage
-----

Go to '/youtube/upload/' to upload video files directly to youtube. When you upload a file, the video entry is created on youtube, 'Video' model that includes video details (video_id, title, etc.) created on your db and a signal sent that you can add your logic to it.
After successful upload, it redirects to the specified page at YOUTUBE_UPLOAD_REDIRECT_URL, if no page is specified, it redirects to the corresponding video page.

Youtube API is integrated to the 'Video' model. To update or delete the 'Video' instance, and model will sync the youtube video up automatically.

Api methods can be used seperately. Please see 'api.py' to get info about methods. Please note that some operations requires authentication. Api methods will not do more than one operations, i.e. will not call authenticate method. So you will need to authenticate manually. Please see 'views.py' for sample implementation.

You can use views for uploading, displaying, deleting the videos.

Signals
-------

The 'video_created' sent after video upload finished and video created successfully. You can also choose to register 'post_save' event of 'Video' model
Following is an example of how you process the signal

    from django_youtube.models import video_created
    from django.dispatch import receiver
    
    @receiver(video_created)
    def video_created_callback(sender, **kwargs):
        """
        Youtube Video is created.
        Not it's time to do something about it
        """
        pass