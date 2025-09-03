In Drupal 9, removing a node translation doesn't automatically delete all related translation revisions because the system prioritizes keeping existing revisions, especially if a pending revision is involved. The deletion process can be complicated by content moderation, which allows multiple revisions of a translation, and a limitation in the Entity/Field API infrastructure where a removed translation in a pending revision is hard to distinguish from a new, unadded one. Consequently, the translation might not be fully deleted, leaving behind its associated revisions in the database until a different approach or patch is applied. 
Why Revisions Aren't Removed

Entity/Field API Limitations:

The underlying Drupal infrastructure for managing entities and fields has a known limitation in distinguishing between a removed translation and a new one added in a pending revision state. 

Content Moderation Complexity:

When content moderation is enabled, a translation can have multiple revisions in different states (e.g., Draft, Published). Deleting the latest published translation doesn't inherently discard all its previous drafts. 

Preserving History:

Drupal is designed to preserve the history of content through revisions. A direct deletion of a translation might only mark it as inactive, rather than removing all its associated historical data. 

How it Manifests

Orphaned Revisions:

You might find the translation marked as deleted in some places, but its revisions still exist and can be accessed in the system. 

UI Issues:

The "Delete translation" feature in the node edit form might not work as expected when multiple revisions are present. 

Solutions and Workarounds

Use the Delete Tab/Button:

For translations with multiple revisions, deleting the translation from the main dashboard or the node's "Delete" tab might be more effective. 

Apply Patches (if applicable):

Specific Drupal issues, such as Drupal.org issue #2940890, addressed the issue of deleting translations in pending revisions by adding the necessary infrastructure to the Entity/Field API. Check for such patches or updates in newer Drupal versions if you're still on Drupal 9. 

Check for Core Bugs:

As of Drupal 9, this was a known limitation, and while workarounds exist, a definitive fix might require a core update or a contributed module to improve the process. 


https://www.drupal.org/project/drupal/issues/2815779

https://www.drupal.org/project/drupal/issues/3006897
