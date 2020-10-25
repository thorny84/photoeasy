# PhotoEasy
PhotoEasy reduces the complexity of requesting an image via the camera, with various settings that can help you customize the event.

Attention, PhotoEasy is optimized for versions from 16 to 30 and uses features that require the presence of a camera, so if you decide to use it in production your application will not be identified in the playStore of devices without a camera.

## Usage
Now, we use the builder to initialize PhotoEasy in the Activity or Fragment in this way:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .build()
```
the only mandatory buider method is `setActivity`, whitout it PhotoEasy generate an `IllegalArgumetException`.
`build()` open the system application camera.

Next step is get a image that we took. Add in `onActivityResult` of activity after the `super`:
```
photoEasy.onActivityResult(requestCode, resultCode, new OnPictureReady() {
   @Override
   public void onFinish(Bitmap thumbnail) {}
});
```
if the creation of the bitmap fails the thumbnail will be `null`.

## MimeType
Supported mime types are: `Jpeg`,`Png`,`Webp`.
To set mime type use buider setter with `PhotoEasy.MimeType`:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .setMimeType(PhotoEasy.MimeType.imagePng)
  .build()
```
Default mime is `Jpeg`

## Image name
If we want to have a specific name for the image file, we can do:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .setPhotoName("image name")
  .build()
```

## Storage Type
We choose where we want to place our image file.</br>
To specify the type of storage use `PhotoEasy.StorageType`:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .setStorageType(PhotoEasy.StorageType.media)
  .build()
```
there are three archiving possibilities in photoeasy:
- `PhotoEasy.StorageType.internal` App-specific internal storage, intended for the exclusive use of your application, not accessible from other applications. From the API 29 images are encrypted.
- `PhotoEasy.StorageType.external` App-specific external storage, these images are available to other applications only with permissions.
- `PhotoEasy.StorageType.media` Storage in multimedia directory, these images are visible from all applications without the need permissions.
Default storage type is `PhotoEasy.StorageType.external`.
Some of these need permissions requested from the user...

## Permissions
Using `PhotoEasy.StorageType.external` or `PhotoEasy.StorageType.media` you need to set permissions to save images, at least up to API 28. PhotoEasy manages permissions internally but if you want to manage them you have to set them:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .enableRequestPermission(false)
  .build()
```
this could generate unexpected exceptions if the permissions are not handled excellently by you.</br>
Leaving the management of permissions to PhotoEasy, but you want control of actions after user choice, you can use:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .setExternalStoragePermission(myExternalStoragePermission)
  .build()
```
extending class:
```
public class ExtStoPer extends ExternalStoragePermission {

  public ExtStoPer(Activity activity) {
    super(activity,RequestMode.alwaysRequest);
  }

  @Override
  public void requestPermissionRationale() {}

  @Override
  public void requestPermission() {}
}
```
`RequestMode` is points you want to have permission control:
- `RequestMode.alwaysRequest` will always ask the user for permission.
- `RequestMode.requestRationalControl` will only give you control the second time the user is asked for permission.
- `RequestMode.allControl` will give you total control.

## Custom directory
With `PhotoEasy.StorageType.media` we can save our image in a custom directory:
```
PhotoEasy photoEasy = PhotoEasy.builder()
  .setActivity(this)
  .setStorageType(PhotoEasy.StorageType.media)
  .saveInCustomDirectory("Custom_directory")
  .build()
```
so to have a personal directory in the gallery application