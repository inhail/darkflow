# darkflow

<br><b>code resouces : https://github.com/thtrieu/darkflow</b></br>
<br><b>labeling tool : https://github.com/tzutalin/labelImg</b></br>
<br><b>dataset preparation referece : https://blog.csdn.net/hysteric314/article/details/54097845</b></br>
<h4>目標：用自己做的dataset，來訓練yolo/darkflow ( 提醒：這是將自己測試過程、遇到的困難都做一個紀錄 )</h4>

<h2>準備dataset：必須要有7種東西，如下說明</h2>  
第1種：要train的影像。我用的是.jpg，影像大小多少都不會影響（至少我的是這樣啦）。放在./darkflow-master/train/Images資料夾中<p>
    <p>
第2種：label.txt檔。裡面是類別名稱，一行寫1個類別名稱，所以如果要train yolo去分辨4種類別，在label.txt中就會有4行。放在./darkflow-master資料夾中<p> 
    <p>
第3種：影像的label資料，必須是.xml檔，所以用labelImg這個工具，一開始就先選好最後是輸出.txt(選YOLO)還是.xml檔(選VOC)。輸出的.xml放在./darkflow-master/train/Annotations資料夾中<p>
    <p>
第4種：.cfg檔。這次我用的是yolo v2，所以必須先到<a href="https://github.com/pjreddie/darknet">darknet</a>下載，在cfg資料夾中找到相對應的.cfg (例如這次用的是yolov2-tiny.cfg)，然後放在./darkflow-master/cfg資料夾中。接著，修改這個檔案的內容及檔名(修改檔名的目的是為了讓自己好記)。在.cfg中最底下有一個<b>[region]</b>區塊，裡面的classes修改成自己要訓練的類別數量（例如這次是4種類別），然後在最後一個<b>[convolutional]</b>修改filter的數量（公式：5*(classes+coords+1)）（例如這次是5*(4+4+1)=45），然後存成自己想要的檔名(這裡用yolov2-tiny-test.cfg)，放在./darkflow-master/cfg資料夾中<p>
<p>
第5種：.weight檔。一樣到darknet下載和.cfg對應的.weight檔，例如這次就要下載yolov2-tiny.weight，放在./darkflow-master/bin資料夾中<p>
    <p>
第6種：.names檔。在原本darkflow資料夾中沒有data這個資料夾，所以自己新增一個./darkflow-master/data，在data資料夾中新增一個記事本，裡面放的內容跟label.txt一樣，只是最後要存成.names檔。(這次存成test.names)<p>
    <p>
第7種：.data。到darknet的data資料夾中下載voc.data，然後修改裡面的內容。第一行的'classes = 20'數量修改成要訓練的類別數量、第四行的'names = data/voc.names'修改成'names = data/test.names'(test這個名稱是在上一步驟的.names檔的名稱，所以不一定每個人都要一樣)，存成自己想要的檔名(這次存成test.data)後放到./darkflow-master/cfg資料夾中。<p>
    
<h2>使用darkflow</h2> 
darkflow所有的使用方法可以直接參考<a href="https://github.com/thtrieu/darkflow">darkflow</a>網頁，只是要注意一點，在有用到flow這個功能的時候，如果跟我一樣是在命令提示字元下使用的，就要改成'python ./flow'。在這裡只記錄安裝及訓練這2個動作的流程(都在cmd下進行下列動作)<p>
<p>
1. <b>安裝：</b><p>  
    d:<p>                                                                                                                                       cd D:/darkflow-master<p>                                                                                                                   python3 setup.py build_ext --inplace<p>
    <p>
        安裝過程如果遇到"darkflow AssertionError: expect 44948596 bytes, found 44948600"這種錯誤訊息: 在./darkflow-master/darkflow/utils/loader.py中的class weights_walker的self.offset=16改成改成self.offset=20就可以解決<p>
2. <b>查看flow有哪些功能:</b><p>
    python ./flow --h<p>
        <p>
3. <b>使用darknet已經訓練好的model來做圖片辨識</b><p>
    #先initialize model  <p>
    python ./flow --model cfg/yolov2.cfg  <p>
    #再用.cfg和.weights檔來預測  <p>
    python ./flow --model cfg/yolov2.cfg --load bin/yolov2.weights --imgdir sample_img/<p>
    #預測的結果會在./sample_img/out/裡面  <p>
    #想看到預測的結果的bounding box訊息，就要加入json參數  <p>
    python ./flow --model cfg/yolov2.cfg --load bin/yolov2.weights --imgdir sample_img/ --json<p>
4. <b>用自己的dataset訓練：</b><p>
    python ./flow --model cfg/yolov2-tiny-test.cfg --load bin/yolov2-tiny.weights --train --annotation train/Annotations --dataset train/Images  #(只有用CPU)<p>
    python ./flow --model cfg/yolov2-tiny-test.cfg --load bin/yolov2-tiny.weights --train --annotation train/Annotations --dataset train/Images --gpu 1.0  #(用了100%的GPU)<p>
    # 也可以選擇optimizer  <p>
        python ./flow --model cfg/yolov2-tiny-test.cfg --load bin/yolov2-tiny.weights --train --annotation train/Annotations --dataset train/Images --trainer adam<p>
    
<h2>訓練完自己的model之後</h2>  
訓練完自己的model之後，我們可以有底下幾種操作方式來do testing：  <p>
1. 我們會在darkflow資料夾中看到一個ckpt資料夾，裡面有checkpoint和.meta和.profile檔。先產生.pb檔（在darkflow的github有說明.pb檔）再進行預測  <p>
python ./flow --model cfg/yolov2-tiny-test.cfg --load bin/yolov2-tiny.weights --savepb  <p>
python ./flow --pbLoad built_graph/yolov2-tiny-test.pb --metaLoad built_graph/yolov2-tiny-test.meta --imgdir sample_img/  <p>
2. 直接用.cfg和.weights檔進行預測  <p>
python ./flow --model cfg/yolov2-tiny-test.cfg --load bin/yolov2-tiny.weights --imgdir sample_img/  <p>
3. 在上述兩個步驟，都可以透過改變threshold值來提高或降低"model的敏感度"，threshold值越小，model越敏感  <p>
python ./flow --model cfg/yolov2-tiny-test.cfg --load bin/yolov2-tiny.weights --imgdir sample_img/ --threshold 0.25<p>
  
  <h2>在訓練的過程或訓練完後，可能會有一些不滿意的or error發生，可以看這幾個討論串</h2>  
  https://github.com/thtrieu/darkflow/issues/80  <p>
  https://github.com/thtrieu/darkflow/issues/473  <p>
  https://github.com/thtrieu/darkflow/issues/462  <p>
  https://github.com/thtrieu/darkflow/issues/149
  
<h2>reading recommandation</h2>  
http://christopher5106.github.io/object/detectors/2017/08/10/bounding-box-object-detectors-understanding-yolo.html
