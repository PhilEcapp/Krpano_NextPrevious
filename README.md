<h2 dir="auto">Krpano Next and Previous</h2>
Krpano - Catching key strokes to move panos Next and Previous. This is the XML code for Krpano.  
<pre dir="auto">

<events devices="desktop" onkeydown="keydown();" onkeyup="keyup();" />

  <action name="keydown">

    showlog();
    trace('keycode=',keycode);

    if(keycode == 37, set(hlookat_moveforce,-1));
    if(keycode == 39, set(hlookat_moveforce,+1));
    if(keycode == 38, set(vlookat_moveforce,-1));
    if(keycode == 40, set(vlookat_moveforce,+1));
    if(keycode == 16, set(fov_moveforce,-0.2));
    if(keycode == 17, set(fov_moveforce,+0.2));

    if(keycode == 100, prevscene());
    if(keycode == 102, nextscene());

  </action>

  <action name="keyup">

    showlog();
    trace('keycode=',keycode);

    if(keycode == 37, set(hlookat_moveforce,0));
    if(keycode == 39, set(hlookat_moveforce,0));
    if(keycode == 38, set(vlookat_moveforce,0));
    if(keycode == 40, set(vlookat_moveforce,0));
    if(keycode == 16, set(fov_moveforce,0));
    if(keycode == 17, set(fov_moveforce,0));

  </action>

  <action name="prevscene">
    if(%1 != findnext, sub(i,scene.count,1));
    txtadd(scenexml,'<krpano>',get(scene[%i].content),'</krpano>');
    if(scenexml == xml.content,
    dec(i);
    if(i LT 0, sub(i,scene.count,1));
    loadscene(get(scene[%i].name), null, MERGE, BLEND(1));
    ,
    dec(i);
    if(i GE 0, prevscene(findnext));
    );
  </action>

  <action name="nextscene">

    set(center_ath,get(view.hlookat));
    set(center_atv,get(view.vlookat));

    if(%1 != findnext, set(i,0));
    txtadd(scenexml,'<krpano>',get(scene[%i].content),'</krpano>');
    if(scenexml == xml.content,
    inc(i);
    if(i == scene.count, set(i,0));
    loadscene(get(scene[%i].name), null, MERGE, BLEND(1));
    lookat(get(center_ath), get(center_atv), 100);
    ,
    inc(i);
    if(i LT scene.count, nextscene(findnext));
    );
  </action>

</pre>
