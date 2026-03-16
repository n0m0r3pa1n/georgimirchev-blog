---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:22"
_publicize_job_id: "43823065167"
author: gmirchev90
categories:
  - android
date: "2020-05-04T12:46:32+00:00"
guid: http://georgimirchev.com/?p=146
parent_post_id: null
post_id: "146"
summary: My problem was that when a user clicks the Sign Up for example button I wanted to validate a field if it contains a valid email and after that dismiss or keep the dialog showing.
tags:
  - dialog
timeline_notification: "1588596396"
title: Android prevent a dialog from closing when a button is clicked
url: /2020/05/04/android-prevent-a-dialog-from-closing-when-a-button-is-clicked/

---
My problem was that when a user clicks the Sign Up for example button I wanted to validate a field if it contains a valid email and after that dismiss or keep the dialog showing.

To achieve that I just copied the code from here which overrides the click of the buttons:

[http://stackoverflow.com/questions/2620444/how-to-prevent-a-dialog-from-closing-when-a-button-is-clicked](http://stackoverflow.com/questions/2620444/how-to-prevent-a-dialog-from-closing-when-a-button-is-clicked)

```
final AlertDialog d = new AlertDialog.Builder(context)
        .setView(v)
        .setTitle(R.string.my_title)
        .setPositiveButton(android.R.string.ok, null) //Set to null. We override the onclick
        .setNegativeButton(android.R.string.cancel, null)
        .create();

d.setOnShowListener(new DialogInterface.OnShowListener() {

    @Override
    public void onShow(DialogInterface dialog) {

        Button b = d.getButton(AlertDialog.BUTTON_POSITIVE);
        b.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View view) {
                // TODO Do something

                //Dismiss once everything is OK.
                d.dismiss();
            }
        });
    }
```
