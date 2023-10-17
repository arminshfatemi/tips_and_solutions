# Docker file number 1
***
### speed up pip installing in image building:
- installing pip packages in image building can be slow because everytime it downloads them
- because of that we need to use our local machine (our pc) pip cache , we mount the pip cache 
to docker container and then when we are installing packages it uses caches
- command below is for mounting the pip cache
- its better for it to be in first in RUN (in my case it cuse bugs when it was not first)
```dockerfile
     RUN --mount=type=cache,target=/root/.cache/pip  pip install -r  requirements.txt 
```

***
### Release expired bug in docker image building
- in some cases i had a bug that when using apt or apt-get to install somethings
in container it fails error below :
-  (Release file for X is not valid yet (invalid for another Xh Xmin Xs). Updates for this repository will not be applied.)
- TIP: one of the ways is to check system bios clock and date and time, 
also check OS date and time
- if it didn't work put below code before and apt command
```dockerfile
    echo "Acquire::Check-Valid-Until \"false\";\nAcquire::Check-Date \"false\";" | cat > /etc/apt/apt.conf.d/10no--check-valid-until
```
