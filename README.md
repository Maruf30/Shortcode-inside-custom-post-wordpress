function post_list_shortcode($atts){
    extract( shortcode_atts( array(
        'count' => '',
    ), $atts) );
     
    $q = new WP_Query(
        array('posts_per_page' => $count, 'post_type' => 'posttype', 'orderby' => 'menu_order','order' => 'ASC')
        );      
         
    $list = '<div class="custom_post_list">';
    while($q->have_posts()) : $q->the_post();
        $idd = get_the_ID();
        $custom_field = get_post_meta($idd, 'custom_field', true);
        $post_content = get_the_content();
        $list .= '
        <div class="single_post_item">
            <h2>' .do_shortcode( get_the_title() ). '</h2>
            '.wpautop( $post_content ).'
            <p>'.$custom_field.'</p>
        </div>
        ';        
    endwhile;
    $list.= '</div>';
    wp_reset_query();
    return $list;
}
add_shortcode('post_list', 'post_list_shortcode');  
