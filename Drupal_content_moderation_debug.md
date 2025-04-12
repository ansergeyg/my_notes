We have been having this bug for over 8 months now.
We have been using the module_moder_status for quite some time now and never had issues with it.
Until one day as a regular procedure to update our core/modules before releasing the project we noticed some functionl regression.
After saving a node the initial status of the node should be 'draft' but it started to save the node in the 'publushed' state.
Now when you save a node in the "draft" mode it redirects you to the "View Draft" page after that. This page has its own moderation state form
where you can change the state of the node from, say, "draft" to "published".
But in our case we no longer could see the "View Draft" mode, because when the node was saved in the "published" state the redirect was to the "View Published" page.

Now the full explanation of why it happens:
For me two, it was confusing a bit that I thought that NodeForm in drupal is the same as Moderation State form and this brought a lot of unnecessary debugging.
Anyways:

NodeForm in Edit mode is not the moderation state form you see in the Draft View mode.
When you programmatically remove #access from the moderation_state field widget on xxx_corporate_workflow_form_alter hook alter
it only hides the whole moderation_state field widget in the Edit Mode of the node.
My assumption was that it also hides the moderation_state field widget in the moderation state form on the Draft View mode which is false.
The moderation state form doesn't use any widgets. It just renders form fields which use the same moderation state data and nothing more.
Now, why then it doesn't work on the Digital Strategy website?
The debugging shows that we use this patch:
```
           "drupal/core": {
                "New non translatable field on translatable content throws error": "some.patch"
            },
```

Which alters the WidgetBase.php file in such a way that when our moderation_state field widget gets processed,
it cannot pass this code:
```
+      $widget = NestedArray::getValue($form, $field_state['array_parents']);
+      if (Element::isVisibleElement($widget)) {
+        $items->setValue($values);
+        $items->filterEmptyItems();
+      }
```
Because as was mentioned above: 

You can programmatically remove #access from the moderation_state field widget on xxx_corporate_workflow_form_alter hook alter
specifically not to show it in the Node Edit mode.

When the moderation_state field widget doesn't set new values to the field the field keeps it's previous state. If the previous state wasw "published"
then after the content moderation module submit handler processes the form it redirects it to the:
View Published page

and NOT to the View Draft page (content_moderation.workflows:node.latest_version_tab) which is the CORRCT one.
