# FirestoreDocument-Android Quickstart

## Introduction

FirestoreDocument-Android it's an Android library for **getting the size of Cloud Firestore documents**. The use of this library will help you prevent your Firestore writes to fail. You'll always be able to check against [the maximum of **1 MiB** (1,048,576 bytes) quota](https://firebase.google.com/docs/firestore/quotas#collections_documents_and_fields).

## Getting Started

To be able to use this library, you need to add [JitPack.io](https://jitpack.io/) repository into your build.gradle (Project) file.

    allprojects {
        repositories {
            ...
            maven {
                url 'https://jitpack.io'
            }
        }
    }

And the latest dependency into your build.gradle (app) file.

    implementation 'com.github.alexmamo:FirestoreDocument-Android:0.1.5'
    
In your Android project, simply create a new instance of the `FirestoreDocument` class:

    FirestoreDocument firestoreDocument = FirestoreDocument.getInstance();
    
Inside this class, there is a `getSize()` method, which will return as an integer the actual size, when you pass a `DocumentSnapshot` object as an argument.

    int documentSize = firestoreDocument.getSize(documentSnapshot);
    
Using the following reference:

    FirebaseFirestore rootRef = FirebaseFirestore.getInstance();
    DocumentReference myTaskIdRef = rootRef.collection("users").document("jeff").collection("tasks").document("my_task_id");
    
As is it also specified in the offcial documentation regarding [the storage size of a Firestore document](https://firebase.google.com/docs/firestore/storage-size), we can take as an example a document with the following structure:

![Firestore Document Structure](doc_structure.png)

To get the size of this document, only the following lines of code are needed:

    myTaskIdRef.get().addOnCompleteListener(task -> {
        if (task.isSuccessful()) {
            DocumentSnapshot document = task.getResult();
            if (document.exists()) {
                int documentSize = firestoreDocument.getSize(document);
                Log.d("TAG", "documentSize: " + documentSize);
            }
        }
    });
    
The result in the logcat will be:

    documentSize: 147
    
The library works as well with queries. So instead of passing a `documentSnapshot`, you can also pass a `queryDocumentSnapshot` object as an argument:

    int documentSize = firestoreDocument.getSize(queryDocumentSnapshot);
    
Note: All documents size returned by this library are in bytes, the example above shows 147 bytes
