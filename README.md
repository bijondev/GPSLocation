# GPSLocation
Android get Latitude and Longitude sample

To get the latitude and longitude of the device's current location in an Android app, you can use the Location API provided by Google Play Services. Here is an example of how to do this:

    Add the following permissions to your app's AndroidManifest.xml file:

<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

    Create a LocationRequest object and set the desired parameters, such as the desired update interval:

LocationRequest locationRequest = new LocationRequest();
locationRequest.setInterval(10000);
locationRequest.setFastestInterval(5000);
locationRequest.setPriority(LocationRequest.PRIORITY_HIGH_ACCURACY);

    Create a LocationSettingsRequest object and add the LocationRequest object to it:

LocationSettingsRequest.Builder builder = new LocationSettingsRequest.Builder()
        .addLocationRequest(locationRequest);

    Check if the location settings are satisfied:

SettingsClient settingsClient = LocationServices.getSettingsClient(this);
Task<LocationSettingsResponse> task = settingsClient.checkLocationSettings(builder.build());

task.addOnSuccessListener(this, new OnSuccessListener<LocationSettingsResponse>() {
    @Override
    public void onSuccess(LocationSettingsResponse locationSettingsResponse) {
        // All location settings are satisfied. The client can initialize
        // location requests here.
    }
});

task.addOnFailureListener(this, new OnFailureListener() {
    @Override
    public void onFailure(@NonNull Exception e) {
        if (e instanceof ResolvableApiException) {
            // Location settings are not satisfied, but this can be fixed
            // by showing the user a dialog.
            try {
                // Show the dialog by calling startResolutionForResult(),
                // and check the result in onActivityResult().
                ResolvableApiException resolvable = (ResolvableApiException) e;
                resolvable.startResolutionForResult(MainActivity.this,
                        REQUEST_CHECK_SETTINGS);
            } catch (IntentSender.SendIntentException sendEx) {
                // Ignore the error.
            }
        }
    }
});

    If the location settings are satisfied, you can request the device's current location:

FusedLocationProviderClient fusedLocationClient = LocationServices.getFusedLocationProviderClient(this);
fusedLocationClient.getLastLocation()
        .addOnSuccessListener(this, new OnSuccessListener<Location>() {
            @Override
            public void onSuccess(Location location) {
                // Got last known location. In some rare situations this can be null.
                if (location != null) {
                    double latitude = location.getLatitude();
                    double longitude = location.getLongitude();
                }
            }
        });

I hope this helps give you an idea of how to get the latitude and longitude of the device's current location in an Android app! Let me know if you have any questions.
