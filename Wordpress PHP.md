# Add tag to enqueued script:
Add this code after the handle is registered in functions.php file:
```
add_filter('script_loader_tag', 'add_tag', 10, 3);
function add_tag($tag, $handle, $src)
 {
 	if ($handle != 'handle-name') {
 		return $tag;
 	}

 	$tag = '<script type="application/json" src="' . esc_url($src) . '"></script>';

 	return $tag;
 }
 ```
 
