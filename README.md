# freemaptoolsradius_wp
A quick script too import the csv list of cities and states and counties from the freemap tools radius tool into wordpress

https://pluginsware.com/
https://www.freemaptools.com/find-zip-codes-inside-radius.htm

<?php
$str = <<<TEXT
24060,VA,Montgomery,Blacksburg,5,0
24062,VA,Montgomery,Blacksburg,5,5
24063,VA,Montgomery,Blacksburg,5,5
24061,VA,Montgomery,Blacksburg,5,5
24128,VA,Giles,Newport,5,9
24058,VA,Pulaski,Belspring,5,14
24111,VA,Montgomery,Mc Coy,5,15
24136,VA,Giles,Pembroke,5,15
24068,VA,Montgomery,Christiansburg,5,15
24087,VA,Montgomery,Elliston,5,16
23958,VA,Appomattox,Pamplin,5,161
25302,WV,Kanawha,Charleston,5,161
27313,NC,Guilford,Pleasant Garden,5,161
TEXT;

$stateMap = array(
    "VA" => 2,
    "WV" => 3,
    "NC" => 4,
    "TN" => 5,
    "KY" => 6

);
$mapData = array();


$d = explode(PHP_EOL,$str);
foreach ($d as $e){
    $f = explode(",",$e);
    if(!isset($mapData[$f[1]])){$mapData[$f[1]] = array();}
    if(!isset($mapData[$f[1]][$f[2]])){$mapData[$f[1]][$f[2]] = array();}
    $mapData[$f[1]][$f[2]][] = $f[3];
}

foreach ($mapData as $state => $counties){
    $state_id = $stateMap[$state];
    $state_id = wp_insert_term($state,"acadp_locations",  array() );
    foreach ($counties as $county => $cities){
        $county_id = 1;
        $county_id = wp_insert_term($county,"acadp_locations",  array("parent"=>$state_id) );
        foreach ($cities as $city){
            $city_id = wp_insert_term($city,"acadp_locations",  array("parent"=>$county_id) );
        }
    }
}
die("<pre>".print_r($mapData,1));
?>
