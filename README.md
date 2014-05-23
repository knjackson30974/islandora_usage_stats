islandora_usage_stats
=====================

A simple module for Drupal 7 to track views of islandora items

Work based on code from https://github.com/roblib/islandora_scholar_upei and the scholar_tracking module for Drupal 6

Creates new variables in the $variables array to allow templates to display view count and download count of Islandora items.

##Features
- View count is based on IP and comes default with a 5 minute cooldown for repeated requests from the same IP
- Tracks views of all types of objects in the repository
- IP Exclusion list to prevent artifically inflating counts while testing/developing

##Variables available:
- **islandora_download_link:** rewritten to redirect to a transparent page to track downloads
- **times_viewed:** How many times the item has been viewed
- **time_last_viewed:** The datetime of the last recorded view in seconds since the unix epoch, format with a call to date()
- **times_downloaded:** How many times the item has been downloaded
- **time_last_downloaded:**  The datetime of the last download in seconds since the unix epoch, format with a call to date()

##How to use:
- Place the downloaded module in your sites/all/modules folder and enable it
- Configure any excluded IP addresses under Admin > Config > Islandora Usage Stats
- In your theme template files, follow the example below to display tracking information

##Example template file without tracking:
```php
<div class="islandora-pdf-content-wrapper clearfix">
<?php if (isset($islandora_content)): ?>
  <div class="islandora-pdf-content">
    <?php print $islandora_content; ?>
  </div>
  <?php if (isset($islandora_download_link)): ?>
  <div class="islandora-download-link">
    <?php print $islandora_download_link; ?>
  </div>
  <?php endif; ?>
<?php endif; ?>
</div>
```

##Example template file with tracking
```php
<div class="islandora-pdf-content-wrapper clearfix">
<?php if (isset($islandora_content)): ?>
  <div class="islandora-pdf-content">
    <?php print $islandora_content; ?>
  </div>
  <?php if (isset($islandora_download_link)): ?>
  <div class="islandora-download-link">
    <?php print $islandora_download_link; ?>
  </div>
  <?php endif; ?>
<?php endif; ?>
</div>
<?php if (module_exists('islandora_usage_stats')): ?>
</br>
<div class="islandora-usage-stats">
  <p>
    Viewed: <span class="badge"><?php print $times_viewed ?></span> Last viewed: <?php print $time_last_viewed==NULL ? 'Never' : date('g:ia, M d, Y', $time_last_viewed); ?></span>
  </p>
  <p>
    Downloaded: <span class="badge"><?php print $times_downloaded ?></span>
  </p>
</div>  <!-- /.islandora-usage-stats -->
<?php endif; ?>
```


