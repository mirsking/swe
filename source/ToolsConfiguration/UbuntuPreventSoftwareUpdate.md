# Ubuntu防止某软件升级


## Using dpkg

Put a package on hold
```
echo "package hold" | sudo dpkg --set-selections
```

Remove the hold
```
echo "package install" | sudo dpkg --set-selections
```

Displaying the status of your packages
```
dpkg --get-selections
```

Displaying the status of a single package
```
dpkg --get-selections | grep "package"
```


## Using apt

you can hold a package using
```
sudo apt-mark hold package_name
```
and remove the hold with
```
sudo apt-mark unhold package_name
```

## Using aptitude

you can hold a package using
```
sudo aptitude hold package_name
```
and remove the hold with
```
sudo aptitude unhold package_name
```


## Using dselect

With dselect, you just have to enter the [S]elect screen, find the package you wish to hold in its present state, and press = or H. The changes will go live immediately after you exit the [S]elect screen.
