# synology-photos-auto-sort
A script to rename, move and sort images and videos files from a source folder to a target folder.

This script is first writted to be used on a Synology NAS, but it will work on any System where bash and exiftool can be used. That means you can use this script in a Linux computer, MacOS, any other NAS working on Linux based system and even Windows with a Linux subsystem installed via WSL. I tested it on synology DSM6 and Debian 9, 10 and 11 with success.

## How it works?
To work this script need **exiftool** installed on your NAS. The best way to install this tool is to add a new package sources from http://www.cphub.net then you will have exiftool available as a package.

* Create in your NAS a folder which would be the source folder of the script
* Add images and videos inside this folder
* run the script like **./synology-photos-auto-sort.sh /path_of_source_folder /path_of_destination_folder**
  Full exemple : ./synology-photos-auto-sort.sh /volume1/Download/images_temp/ /volume1/photo
* The script will move and rename photos and videos in taget folder like this structure :
  **/Target_folder/YEAR/Year.Month/yearmonthday_hourminutesseconds.jpg**
* If a file with the same name already exist, there is a base64 file comparison to ensure that the content is different, if it is different the image will by copied and get a new name (same as target + unique ID), if the content is the same the image is copied in a duplicate folder
* If exif data is not available, th script will create an error folder inside the source folder and move images and videos inside this error folder
* A logs folder is available with all errors /duplicated listed by sync date
* The filename and path must not have any whitespace !!!

## How to automatise this script?
Access to your NAS with ssh, then clone this repository.

* `ssh user@nas_ip`
* `git clone https://github.com/Gulivertx/synology-photos-auto-sort.git`

Add a new cron job to run periodically the script
* `sudo vi /etc/crontab`
* `* * * * * root /var/services/homes/gulivert/synology-photos-auto-sort/synology-photos-auto-sort.sh /volume1/Download/images_temp /volume1/photo`
* `sudo synoservice -restart crond`

Replace **gulivert** by your username.

This rule will launch the script every minute using as source folder /volume1/Download/images_temp and /volume1/photo as target.

## Jim Notes

* Log into Synology

```console
PS C:\Users\jajoh> ssh jajohga@10.0.0.216 -p22
The authenticity of host '10.0.0.216 (10.0.0.216)' can't be established.
ED25519 key fingerprint is SHA256:j034je7JJ7Pz/GHJNdFUyD7iYYcA6K/UNUabo0IUvPk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?
Host key verification failed.
PS C:\Users\jajoh> ssh jajohga@10.0.0.216 -p22
The authenticity of host '10.0.0.216 (10.0.0.216)' can't be established.
ED25519 key fingerprint is SHA256:j034je7JJ7Pz/GHJNdFUyD7iYYcA6K/UNUabo0IUvPk.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.0.0.216' (ED25519) to the list of known hosts.
jajohga@10.0.0.216's password:
```
## Exiftool
https://exiftool.org/

## Testing
Input: Dana Pictures
out: out

https://www.cyberciti.biz/tips/handling-filenames-with-spaces-in-bash.html

```console
jajohga@jajohga:~/exiftool/Image-ExifTool-12.73$ rm /tmp/synology_photos_auto_sort.pid
jajohga@jajohga:~/exiftool/Image-ExifTool-12.73$ ./synology-photos-auto-sort.sh ./Pictures ../out
```
## PNG
File Modification Date/Time

Total file count: 1401. ?

Total ext count: 1388. right

  out: 985 (moved + UUID)

Total file moved count: 775.

Total file error count: 0. right

Total file duplicates count: 403. right

**Total file moved UUID count: 210.**

See 

"\\jajohga\home\exiftool\Image-ExifTool-12.73\out\2016\2016.07\20160721_204531.jpeg"

