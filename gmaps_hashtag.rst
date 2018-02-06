
Hashtag analysis with gmaps
===========================

*Basic required packages*

.. code:: ipython3

    import pandas as pd
    import gmaps

.. code:: ipython3

    # configure gmaps with your api key
    gmaps.configure(api_key='your api key')

Import and manipulate data
--------------------------

Data from Instagram generic hashtag scraped with the crawler
**`Instagram Scraper <https://github.com/h4t0n/instagram-scraper>`__**

.. code:: ipython3

    data = pd.read_json('myrepo/scrapy_instagram/scraped/hashtag/iconsigliati/06-02-2018_22', lines=True)

.. code:: ipython3

    # taken_at_timestamp is a unix timestamp - we add a column for the date-time
    data['taken_at_timestamp'] = data['taken_at_timestamp'].astype(int)
    data['time'] = pd.to_datetime(data['taken_at_timestamp'], unit='s')

.. code:: ipython3

    # reindex the data frame by new time
    #data = data.set_index('time')

.. code:: ipython3

    data.head() #take a look at the data... But we have some post without location




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>caption</th>
          <th>display_url</th>
          <th>id</th>
          <th>loc_id</th>
          <th>loc_lat</th>
          <th>loc_lon</th>
          <th>loc_name</th>
          <th>owner_id</th>
          <th>owner_name</th>
          <th>shortcode</th>
          <th>taken_at_timestamp</th>
          <th>time</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>0</th>
          <td>Manarola\n\n#manarola #le5terre #lecinqueterre...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/bec...</td>
          <td>1708942384354027008</td>
          <td>0</td>
          <td>0.0</td>
          <td>0.0</td>
          <td></td>
          <td>6288545769</td>
          <td>maria.riccotti</td>
          <td>Be3YgtRB-Ww</td>
          <td>1517941829</td>
          <td>2018-02-06 18:30:29</td>
        </tr>
        <tr>
          <th>1</th>
          <td>Manarola\n\n#manarola #le5terre #lecinqueterre...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/496...</td>
          <td>1708943040116520192</td>
          <td>0</td>
          <td>0.0</td>
          <td>0.0</td>
          <td></td>
          <td>6288545769</td>
          <td>maria.riccotti</td>
          <td>Be3YqP_hXU0</td>
          <td>1517941907</td>
          <td>2018-02-06 18:31:47</td>
        </tr>
        <tr>
          <th>2</th>
          <td>Senza bisogno di tante parole...perché non sem...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/9fd...</td>
          <td>1708942772638358016</td>
          <td>0</td>
          <td>0.0</td>
          <td>0.0</td>
          <td></td>
          <td>5332466571</td>
          <td>__carmelarusso</td>
          <td>Be3YmW4lJo_</td>
          <td>1517941875</td>
          <td>2018-02-06 18:31:15</td>
        </tr>
        <tr>
          <th>3</th>
          <td>#Repost @tizzy181 with @get_repost\n・・・\n#sand...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/580...</td>
          <td>1708955497106812416</td>
          <td>0</td>
          <td>0.0</td>
          <td>0.0</td>
          <td></td>
          <td>5974484114</td>
          <td>amazing.panorama</td>
          <td>Be3bfheAK5C</td>
          <td>1517943392</td>
          <td>2018-02-06 18:56:32</td>
        </tr>
        <tr>
          <th>4</th>
          <td>Pordenone #pordenone #friuliveneziagiulia #uau...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/a02...</td>
          <td>1708972903192997888</td>
          <td>0</td>
          <td>0.0</td>
          <td>0.0</td>
          <td></td>
          <td>4684737539</td>
          <td>merifabbro</td>
          <td>Be3fc0Jli_f</td>
          <td>1517945467</td>
          <td>2018-02-06 19:31:07</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    filtered = data[data['loc_id']!=0]
    filtered.head() # now we have only post with location... we want to visualize them




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>caption</th>
          <th>display_url</th>
          <th>id</th>
          <th>loc_id</th>
          <th>loc_lat</th>
          <th>loc_lon</th>
          <th>loc_name</th>
          <th>owner_id</th>
          <th>owner_name</th>
          <th>shortcode</th>
          <th>taken_at_timestamp</th>
          <th>time</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>7</th>
          <td>Pozzanghere \n#ilgermogliodelticino #volgopavi...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/da7...</td>
          <td>1708949994525244672</td>
          <td>213033286</td>
          <td>45.166700</td>
          <td>9.166670</td>
          <td>Pavia, Italy</td>
          <td>5747475778</td>
          <td>alex.schiappelli</td>
          <td>Be3aPcylT1h</td>
          <td>1517942736</td>
          <td>2018-02-06 18:45:36</td>
        </tr>
        <tr>
          <th>8</th>
          <td>#tramontosulmare🌅#iconsigliati#italy_stop#ital...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/e1a...</td>
          <td>1708945735653771520</td>
          <td>235490003</td>
          <td>40.294650</td>
          <td>17.855253</td>
          <td>Torre Lapillo Beach</td>
          <td>3548521363</td>
          <td>concettamilizia</td>
          <td>Be3ZReaD8jW</td>
          <td>1517942228</td>
          <td>2018-02-06 18:37:08</td>
        </tr>
        <tr>
          <th>9</th>
          <td>Nella penombra c'era una Volta. .. Alessandro\...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/8f0...</td>
          <td>1708945255792293376</td>
          <td>213033286</td>
          <td>45.166700</td>
          <td>9.166670</td>
          <td>Pavia, Italy</td>
          <td>5747475778</td>
          <td>alex.schiappelli</td>
          <td>Be3ZKfgFrWR</td>
          <td>1517942171</td>
          <td>2018-02-06 18:36:11</td>
        </tr>
        <tr>
          <th>10</th>
          <td>"L’arte non è ciò che vedi, ma ciò che fai ved...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/87b...</td>
          <td>1708942637758648832</td>
          <td>245566046</td>
          <td>38.110833</td>
          <td>13.353611</td>
          <td>Cappella Palatina</td>
          <td>4030004715</td>
          <td>giuseppegallo.ist16</td>
          <td>Be3YkZRH5Hj</td>
          <td>1517941859</td>
          <td>2018-02-06 18:30:59</td>
        </tr>
        <tr>
          <th>11</th>
          <td>Manarola\n\n#manarola #le5terre #lecinqueterre...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/937...</td>
          <td>1708948076552803072</td>
          <td>251246786</td>
          <td>44.300000</td>
          <td>9.333330</td>
          <td>Lavagna</td>
          <td>6288545769</td>
          <td>maria.riccotti</td>
          <td>Be3ZziihFNC</td>
          <td>1517942507</td>
          <td>2018-02-06 18:41:47</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    sorted = filtered.sort_values(by=['time'], ascending=False)
    sorted.head()




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>caption</th>
          <th>display_url</th>
          <th>id</th>
          <th>loc_id</th>
          <th>loc_lat</th>
          <th>loc_lon</th>
          <th>loc_name</th>
          <th>owner_id</th>
          <th>owner_name</th>
          <th>shortcode</th>
          <th>taken_at_timestamp</th>
          <th>time</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>13766</th>
          <td>#toblino #igerstrentino #ig_trentinoaltoadige ...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/2ba...</td>
          <td>1708991143273596416</td>
          <td>903279017</td>
          <td>46.050000</td>
          <td>10.960000</td>
          <td>Lago di Toblino</td>
          <td>4583233796</td>
          <td>gio.meco</td>
          <td>Be3jmPjHIZh</td>
          <td>1517947641</td>
          <td>2018-02-06 20:07:21</td>
        </tr>
        <tr>
          <th>13765</th>
          <td>"Nevicata. La metamorfosi del mondo avviene in...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/96f...</td>
          <td>1708989572120516864</td>
          <td>246658712</td>
          <td>46.952166</td>
          <td>11.387351</td>
          <td>Ladurns</td>
          <td>6828038685</td>
          <td>alessandra.belia</td>
          <td>Be3jPYTFxkc</td>
          <td>1517947454</td>
          <td>2018-02-06 20:04:14</td>
        </tr>
        <tr>
          <th>13764</th>
          <td>“Non basta mangiare la foglia per definirsi ve...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/4e5...</td>
          <td>1708988163479123456</td>
          <td>31499759</td>
          <td>41.900000</td>
          <td>12.500000</td>
          <td>Rome, Italy</td>
          <td>195015875</td>
          <td>marco_argentati</td>
          <td>Be3i64ZhTZ3</td>
          <td>1517947286</td>
          <td>2018-02-06 20:01:26</td>
        </tr>
        <tr>
          <th>13762</th>
          <td>Lungomare di Acitrezza\n.\n.\n.\n#italy #igers...</td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/19a...</td>
          <td>1708986237588844288</td>
          <td>1033833149</td>
          <td>37.563611</td>
          <td>15.161389</td>
          <td>Aci Trezza</td>
          <td>4214788243</td>
          <td>unclegix86</td>
          <td>Be3ie2xg67T</td>
          <td>1517947056</td>
          <td>2018-02-06 19:57:36</td>
        </tr>
        <tr>
          <th>13761</th>
          <td></td>
          <td>https://instagram.ffco2-1.fna.fbcdn.net/vp/37b...</td>
          <td>1708981441041652992</td>
          <td>214219534</td>
          <td>44.411100</td>
          <td>8.932800</td>
          <td>Genova, Italy</td>
          <td>209203860</td>
          <td>danige7916</td>
          <td>Be3hZDpBd0R</td>
          <td>1517946485</td>
          <td>2018-02-06 19:48:05</td>
        </tr>
      </tbody>
    </table>
    </div>



.. code:: ipython3

    # get the post of the last day
    last_day = sorted[(sorted['time'] > '2018-02-05')]
    len(last_day) 




.. parsed-literal::

    186



.. code:: ipython3

    # post cleaned for bad NaN on latitude longityude
    last_day_nona = last_day.dropna()
    len(last_day_nona)




.. parsed-literal::

    186



.. code:: ipython3

    full_only_location = filtered[['loc_lat','loc_lon']]
    len(full_only_location)




.. parsed-literal::

    10838



.. code:: ipython3

    # post cleaned for bad NaN on latitude longityude
    full_only_location_nona = full_only_location.dropna()
    len(full_only_location_nona)




.. parsed-literal::

    10833



Play with Maps
--------------

heatmap
-------

.. code:: ipython3

    rome = (41.9, 12.4833)
    fig = gmaps.figure(center=rome, zoom_level=5)
    heatmap_layer = gmaps.heatmap_layer(full_only_location_nona)
    fig.add_layer(heatmap_layer)
    fig



.. raw:: html

    <p>Failed to display Jupyter Widget of type <code>Figure</code>.</p>
    <p>
      If you're reading this message in the Jupyter Notebook or JupyterLab Notebook, it may mean
      that the widgets JavaScript is still loading. If this message persists, it
      likely means that the widgets JavaScript library is either not installed or
      not enabled. See the <a href="https://ipywidgets.readthedocs.io/en/stable/user_install.html">Jupyter
      Widgets Documentation</a> for setup instructions.
    </p>
    <p>
      If you're reading this message in another frontend (for example, a static
      rendering on GitHub or <a href="https://nbviewer.jupyter.org/">NBViewer</a>),
      it may mean that your frontend doesn't currently support widgets.
    </p>



.. code:: ipython3

    # playing with heatmap
    heatmap_layer.max_intensity = 200
    heatmap_layer.point_radius = 10

markers
-------

.. code:: ipython3

    last_day_locations = [(elem.loc_lat, elem.loc_lon) for elem in last_day_nona.itertuples()]
    last_day_locations[0:2] # debug location




.. parsed-literal::

    [(46.05, 10.96), (46.9521664112, 11.3873505592)]



.. code:: ipython3

    # a very simple template with image and owner
    info_box_template = """
    <dl>
    <dt>Owner</dt><dd>{owner_name}</dd>
    <dd>{caption}<dd>
    </dl>
    <img src="{display_url}">
    """

.. code:: ipython3

    location_info = [info_box_template.format(**elem) for i,elem in last_day_nona.to_dict('index').items()]
    location_info[0:2] # debug the template




.. parsed-literal::

    ['\n<dl>\n<dt>Owner</dt><dd>gio.meco</dd>\n<dd>#toblino #igerstrentino #ig_trentinoaltoadige #ig_italia #whatitalyis #awesomeitalia #italia_bestphoto #iconsigliati #ilikeitaly #yellersitalia #top_italia_foto #snapitaly #italyvisuals #gardaoutdoors #italia_dev #italyexplorer #topitalyphoto #gardaemotion #spot_italy #europestyle_ #italiastyle20 #foto_italiane #italiainunoscatto #thehub_italia #paesaggitalia #italy_stop #volgotrentino #trentino #italyvisuals #followme<dd>\n</dl>\n<img src="https://instagram.ffco2-1.fna.fbcdn.net/vp/2baf0debe941fbe6898e889f54bb30b8/5B24E45B/t51.2885-15/e35/27573960_2055508131396340_1262266110628069376_n.jpg">\n',
     '\n<dl>\n<dt>Owner</dt><dd>alessandra.belia</dd>\n<dd>"Nevicata. La metamorfosi del mondo avviene in silenzio."\n(Heinrich Wiesner)\n\nLadurns (BZ), 2 febbraio 2018\n\n#sciare #montagna #snow #loves_montains #inverno2018 #settimanabianca #dolomitisuperski #ladurns #bolzano #altoadige #südtirol #italy #italiainunoscatto #shotz_of_italia #iconsigliati #italialike #fotografandolitalia #new_photoitalia #scatti_italiani #altoadigeweb #italia365 #volgoitalia #volgotrentinoaltoadige #yallersitalia #yallerstrentino_altoadige #altoadigedascoprire #igerstrentinoaltoadige #paesaggi_italiani #loves_trentinoaltoadige #italia360gradi<dd>\n</dl>\n<img src="https://instagram.ffco2-1.fna.fbcdn.net/vp/96ff023190c6eb15f492e8e2c85b0091/5B1F11C9/t51.2885-15/e35/26872799_394543204291613_2025185652542275584_n.jpg">\n']



.. code:: ipython3

    # another way to interpolate the template by using itertuples
    location_info_2 = [info_box_template.format(**elem._asdict()) for elem in last_day_nona.itertuples()]
    location_info_2[0:2] # debug the template




.. parsed-literal::

    ['\n<dl>\n<dt>Owner</dt><dd>gio.meco</dd>\n<dd>#toblino #igerstrentino #ig_trentinoaltoadige #ig_italia #whatitalyis #awesomeitalia #italia_bestphoto #iconsigliati #ilikeitaly #yellersitalia #top_italia_foto #snapitaly #italyvisuals #gardaoutdoors #italia_dev #italyexplorer #topitalyphoto #gardaemotion #spot_italy #europestyle_ #italiastyle20 #foto_italiane #italiainunoscatto #thehub_italia #paesaggitalia #italy_stop #volgotrentino #trentino #italyvisuals #followme<dd>\n</dl>\n<img src="https://instagram.ffco2-1.fna.fbcdn.net/vp/2baf0debe941fbe6898e889f54bb30b8/5B24E45B/t51.2885-15/e35/27573960_2055508131396340_1262266110628069376_n.jpg">\n',
     '\n<dl>\n<dt>Owner</dt><dd>alessandra.belia</dd>\n<dd>"Nevicata. La metamorfosi del mondo avviene in silenzio."\n(Heinrich Wiesner)\n\nLadurns (BZ), 2 febbraio 2018\n\n#sciare #montagna #snow #loves_montains #inverno2018 #settimanabianca #dolomitisuperski #ladurns #bolzano #altoadige #südtirol #italy #italiainunoscatto #shotz_of_italia #iconsigliati #italialike #fotografandolitalia #new_photoitalia #scatti_italiani #altoadigeweb #italia365 #volgoitalia #volgotrentinoaltoadige #yallersitalia #yallerstrentino_altoadige #altoadigedascoprire #igerstrentinoaltoadige #paesaggi_italiani #loves_trentinoaltoadige #italia360gradi<dd>\n</dl>\n<img src="https://instagram.ffco2-1.fna.fbcdn.net/vp/96ff023190c6eb15f492e8e2c85b0091/5B1F11C9/t51.2885-15/e35/26872799_394543204291613_2025185652542275584_n.jpg">\n']



.. code:: ipython3

    marker_layer = gmaps.marker_layer(last_day_locations, info_box_content=location_info)

.. code:: ipython3

    fig2 = gmaps.figure()
    fig2.add_layer(marker_layer)
    fig2



.. raw:: html

    
.. raw:: html

    <img src='./images/ht-m2.png'> 
    <img src='./images/ht-m3.png'>

