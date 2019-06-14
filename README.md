# Presentation Notes and Slides

## About

This repo contains the slides and resources for an introductory presentation in to using Open edX.

The slides are made using [reveal.js](https://github.com/hakimel/reveal.js/)

## Resources

### Course Backup Script

```
#!/bin/bash

echo "Pulling Course IDs..."
COURSE_IDS="$(/edx/bin/python.edxapp -W ignore /edx/app/edxapp/edx-platform/manage.py lms dump_course_ids --settings aws)"

for COURSE in $COURSE_IDS
do
   echo "Start Backup of $COURSE"

   if [ ! -d "/edx/course_backups/$COURSE" ]; then
      mkdir /edx/course_backups/$COURSE
   fi

   /edx/bin/python.edxapp -W ignore /edx/bin/manage.edxapp cms --settings=aws export "$COURSE" /edx/course_backups/$COURSE/ 2>&1 >  /dev/null
   echo "End Backup of $COURSE"

done
```

### Example cronjob Entry

```
  # Backup All EdX Courses 2:00AM
0 2 * * * /root/edx_backup_course.sh 2>&1
```

### Example Site Configuraiton

```
{
  "course_email_from_addr":"courses@online.unsupported.io",
  "university":"Online University",
  "PLATFORM_NAME":"Online Demo Program",
  "email_from_address":"courses@online.unsupported.io",
  "SITE_NAME":"online.unsupported.io",
  "site_domain":"online.unsupported.io",
  "SESSION_COOKIE_DOMAIN":"online.unsupported.io",
  "platform_name":"online.unsupported.io"
}
```
