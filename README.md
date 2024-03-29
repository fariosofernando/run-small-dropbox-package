<img height="302px" width="302px" src="https://github.com/fariosofernando/run-small-dropbox-package/raw/master/git_assets/run-small-dropbox_icone.png">

# Run Small Dropbox Package

A Dart SDK that simplifies interaction with the Dropbox API.
This SDK provides convenient abstractions and methods for common operations when working with Dropbox, such as copying files, creating folders, downloading files, and more.

The goal is to accelerate the integration of Dropbox functionality into the Dart and Flutter apps.

The SDK is designed to be easy to use and follows a similar structure to other popular SDKs like Firebase, making it easy for familiar developers to seamlessly integrate Dropbox functionality into their apps.

## Update (3.1.0)

In some cases, you may have file or folder names with non-ASCII characters. The `escapeNonAscii` (released in version 3.1.0) function was designed to handle situations where you may encounter file or folder names containing non-ASCII characters when interacting with the Dropbox API. The Dropbox API requires these characters to be handled appropriately to ensure smooth communication.

```dart
void main() {
  var escapedString = escapeNonAscii('Café');
  print('Escaped String: $escapedString'); // >>> Escaped String: Caf\u00e9
}
```

> escapeNonAscii is a built-in function of Run Small Dropbox, you don't need to import or define it explicitly.

## Getting Started

To use this package, add run_small_dropbox as a dependency in your pubspec.yaml file. For example:

```yaml
dependencies:
  run_small_dropbox: ^3.0.2
```

## Features

- File Operations: Copy, move, delete, and manage files and folders.

- Batch Operations: Perform multiple file operations in a single batch.

- Folder Creation: Create folders individually or in batches.

- Download and Upload Files: Download files, download folders as ZIP, upload files, and more.

- Metadata Retrieval: Retrieve metadata for files and folders.

- File and Folder Search: Search for files and folders.

- Paper Operations: Create and update Paper documents.

## Usage

You need to get a DropboxApp instance to be able to use Dropbox modules, you can do it like this:

```dart

final app = Dropbox.initializeApp('YOUR-ACCESS-TOKEN');

```

> NOTE: "Modules" are parts of the SDK that you can access separately.
> They would basically be what Firebase Storage, Firebase Core, and
> Firebase Authentication are to the Firebase SDK.

So far we only have a single “Module”, DropboxFile.

All “Modules” depend on a DropboxApp object to function.

> I won't put comments over the examples, you can use your IDE to get an idea of what each method does. All methods have rich documentation.

### Copy File

```dart

Feature<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile = DropboxFile(yourDropboxAppInstance);

    var data = await dropboxFile.copyFile(relocationPath: RelocationPath(fromPath: '/Documents/favicon2_.png', toPath: '/Desktop/favicon2_.png'));
}

```

In the examples, you will probably notice that in some cases, the beginning of folders begins with a forward slash "/". What happens is that the Dropbox API returns an exception if it doesn't find this bar at the beginning. But Run Small Dropbox gives you the freedom to add it or not, thanks to `rPath`:

```dart
void main(){
    String path1 = rPath('/MyPathInDropbox'); // With the forward slash "/"
    String path2 = rPath('MyPathInDropbox'); // Without the slash "/".

    print("output 1: $path1"); // >>> output 1: /MyPathInDropbox
    print("output 2: $path2"); // >>> output 2: /MyPathInDropbox
}
```

> rPath is an internal function of Run Small Dropbox, you do not need to import it or define it explicitly. When using a method with requires a `path`, you can choose to pass or not pass without the "/" slash.
> `dropboxFile.copyFile(FolderWithoutSlash/file.txt);`

### Copy File Batch

```dart

Feature<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile = DropboxFile(yourDropboxAppInstance);

    List<RelocationPath> entries = [
        RelocationPath(fromPath: '/Documents/drop_up.txt', toPath: '/Desktop/drop_up.txt'),
        RelocationPath(fromPath: '/Documents/uploadtestfile.txt', toPath: '/Desktop/uploadtestfile.txt'),
      ];

      var data = await dropboxFile.copyBatch(entries: entries);
}

```

### Copy Batch Check

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);
    var data =  await dropboxFile.copyBatchCheck('dbjid:AAAeL_0EYiPPSNJabLJLqdAYWSzDBghRkbYWoV2SDx9Lbg7rr9PG8_Igr_7_irFzWvaVNTeWU7r8lab3cJ9sUOsA');

}

```

### Get Copy Reference

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);
    var data =  await dropboxFile.getCopyReference('/Documents/orders.json');
}

```

### Save Copy Reference

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile = DropboxFile(yourDropboxAppInstance);
    String copyReference = 'AAAAAlDqCCBkYTZldjZoa3ZiOW0', path = '/Documents/ref/orders.json';

    var data = await dropboxFile.saveCopyReference(copyReference, path);
}

```

### Create Folder

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.createFolder('Downloads', autorename:  true);
}

```

### Create Folder Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.createFolderBatch(
        ['/Lib','Bin'],
        autorename:  false,
        forceAsync:  true,
    );

}

```

### Check Create Folder Batch Job Status

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    // For use this method, the flag forceAsync in createFolderBatch must be true
    String asyncJobId =  'dbjid:AACIUTFyJ9SsVq7tFezji8rK-sO4GdpJ2wcn5hOUwsEw71d7hfa2aw_r9SCyiE9rxFqkwvIXmiNo5_KLTY8_LezA';

    var data =  await dropboxFile.checkCreateFolderBatchJobStatus(asyncJobId);
}

```

### Delete File or Folder

```dart

Future<void> method() async {
    // Get DropboxFile Instance

    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.deleteFileOrFolder('/Lib');
}

```

### Delete Files Batch

```dart

Future<void> method() async {
// Get DropboxFile Instance
DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

var data =  await dropboxFile.deleteFilesBatch([
        DeleteArg(path:  'Downloads (1)'),
        DeleteArg(path:  'Downloads (2)'),
    ],
);

}

```

### Delete Batch Check

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    String asyncJobId =  'dbjid:AABWTKVCz9BVnnCDelfli-hJO8ve91g7lszKeV7WmycG5kRb5F1X6kTjhYJDTekMK6IDRHZXV7R3UBY417cseFZd';

    var data =  await dropboxFile.deleteBatchCheck(asyncJobId);

}

```

### Download File

```dart

Future<void> method() async {
// Get DropboxFile Instance
DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

var data =  await dropboxFile.downloadFile('/Documents/orders.json');
    if (data['success']) {
        var file =  File('Me/Path/file.json');
        await file.writeAsBytes(utf8.encode(data['result']));
    }
}

```

### Download Folder as Zip

```dart

Future<void> method() async {
// Get DropboxFile Instance
DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

var data =  await dropboxFile.downloadFolderAsZip('/Documents');

    if (data['success']) {
        var file =  File('Me/Path/file.zip');

        await file.writeAsBytes(utf8.encode(data['result']));
    }
}

```

### Export File

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.exportFile('/Documents/Prime Factorization.xlsx');
}

```

### Get File Lock Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.getFileLockBatch([]);
}

```

### Get Metadata

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.getMetadata('Desktop/orders.json');
}

```

### Get Preview

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.getPreview('/Documents/mitologiagrega.docx');
}

```

### Get Temporary Link

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.getTemporaryLink('/Documents/drop_up.txt');
}

```

### Get Temporary Upload Link

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);
    var data =  await dropboxFile.getTemporaryUploadLink('/Documents/favicon2_.png');
}

```

### Get Thumbnail V2

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);
    var data =  await dropboxFile.getThumbnailV2('/Documents/favicon2_.png');

    if (data['success']) {
        var file =  File('/Me/Desktop/favicon2_.jpeg');
        await file.writeAsBytes(utf8.encode(data['result']));
    }
}

```

### Get Thumbnail Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.getThumbnailBatch(['/Documents/favicon2_.png', '/Documents/favicon2_.png']);
}

```

### List Folder

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.listFolder('/Documents');
}

```

### List Folder Continue

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    String cursor =  'AAE78DDMQjfST3HwI';

    var data =  await dropboxFile.listFolderContinue(cursor);
}

```

### List Folder Get Latest Cursor

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.listFolderGetLatestCursor('/Documents');
}

```

### List Folder Longpoll

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    String cursor =  'AAGvWI9NjRfdjJwFCJdlQKIbRNn';
    int timeout =  30;
    var data =  await dropboxFile.listFolderLongpoll(cursor, timeout: timeout);
}

```

### List Revisions

```dart

Future<void> method() async {

    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.listRevisions('/Documents/mitologiagrega.docx');
}

```

### Lock File Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.lockFileBatch(['/Desktop/orders.json']);
}

```

### Move V2

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.moveV2(RelocationPath(fromPath:  '/Documents/users.json', toPath:  '/Desktop/users.json'));
}

```

### Move Batch V2

```dart

Future<void> method() async {

    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.moveBatchV2([
            RelocationPath(fromPath:  '/Documents/users.json', toPath:  '/Desktop/users.json'),
            RelocationPath(fromPath:  '/Documents/main.dart', toPath:  '/Desktop/main.dart'),
        ],
    );
}

```

### Move Batch Check V2

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    String asyncJobId =  'dbjid:AACUbbYLC4V4a58VE3L923XCBH5myG1_qK33mhOjwS5p-7YxylZCpnhcLMlCNCmEA5oQAJMJIYQZkaLee05zMIXW';

    var data =  await dropboxFile.moveBatchCheckV2(asyncJobId);
}

```

### Paper Create

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.paperCreate(
        '/Desktop/document.paper',
        importFormat:  ImportFormat.html,
        );
}

```

### Paper Update

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.paperUpdate('');
}

```

### Permanently Delete

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.permanentlyDelete('/Desktop/orders.json');
}

```

### Restore File

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.restoreFile('/Documents/file.txt', '/Documents/file.txt');
}

```

### Save URL

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.saveUrl('', '');
}

```

### Check Job Status

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.checkJobStatus('');
}

```

### Search Files

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.searchFiles('orders');
}

```

### Search Continue

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.searchContinue('');
}

```

### Add Tag

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.addTag('/Desktop/orders.json', 'sdkTest');
}

```

### Get Tags

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.getTags(['/Desktop/orders.json']);
}

```

### Remove Tag

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.removeTag('/Desktop/orders.json', 'sdkTest');
}

```

### Unlock File Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.unlockFileBatch(['/Desktop/orders.json']);
}

```

### Upload File

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.uploadFile(File('/Me/Desktop/dev_ttmp/main.dart'), destinationPath:  '/Documents/main.dart');
}

```

### Upload Session Append

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.uploadSessionAppend(File(''), sessionID:  '', offset:  3);
}

```

### Upload Session Finish

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.uploadSessionFinish('', 0, '');
}

```

### Upload Session Finish Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    List<UploadSessionFinishArg> entries = [
        UploadSessionFinishArg(
            cursor:  UploadSessionCursor(sessionId:  '', offset:  0),
            commit:  CommitInfo(
                path:  '/Homework/math/Matrices.txt',
                mode:  WriteMode.add,
                autorename:  true,
                clientModified:  DateTime.now(),
                mute:  false,
                propertyGroups: [
                    PropertyGroup(
                        templateId: 'your_template_id',
                        fields: [
                            PropertyField(name:  'name', value:  'Bob'),
                            ]
                        ),
                    ],
                strictConflict:  false,
                contentHash:  'contentHash123',
            ),
        ),
    ];

    var data =  await dropboxFile.uploadSessionFinishBatch(entries);
}

```

### Upload Session Finish Batch Check

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.uploadSessionFinishBatchCheck('');
}

```

### Upload Session Start

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.uploadSessionStart(File(''));
}

```

### Upload Session Start Batch

```dart

Future<void> method() async {
    // Get DropboxFile Instance
    DropboxFile dropboxFile =  DropboxFile(yourDropboxAppInstance);

    var data =  await dropboxFile.uploadSessionStartBatch(0);
}

```

_Ensure to replace `yourDropboxAppInstance` with the appropriate initialization of your DropboxApp instance._

## BYBY

We hope this SDK makes your interactions with the Dropbox API easier when developing your Dart/Flutter projects.

If you have any questions, suggestions or encounter any problems, feel free to get in touch.

You can visit the repository on [Github](https://github.com/fariosofernando/run-small-dropbox-package) to learn more about it. Happy coding!

Hey! Run, run! Small Dropbox!
