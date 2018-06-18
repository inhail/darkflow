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
第4種：.cfg檔。這次我用的是yolo v2，所以必須先到<a href="https://github.com/pjreddie/darknet">darknet</a>下載，在cfg資料夾中找到相對應的.cfg (例如這次用的是yolov2-tiny.cfg)，然後放在./darkflow-master/cfg資料夾中。接著，修改這個檔案的內容及檔名(修改檔名的目的是為了讓自己好記)。在.cfg中最底下有一個<b>[region]</b>區塊，裡面的classes修改成自己要訓練的類別數量（例如這次是4種類別），然後在最後一個<b>[convolutional]</b>修改filter的數量（公式：5*(classes+coords+1)）（例如這次是5*(4+4+1)=45），然後存成自己想要的檔名，放在./darkflow-master/cfg資料夾中<p>
<p>
第5種：.weight檔。一樣到darknet下載和.cfg對應的.weight檔，例如這次就要下載yolov2-tiny.weight，放在./darkflow-master/bin資料夾中<p>
    <p>
第6種：.names檔。在原本darkflow資料夾中沒有data這個資料夾，所以自己新增一個./darkflow-master/data，在data資料夾中新增一個記事本，裡面放的內容跟label.txt一樣，只是最後要存成.names檔。(這次存成test.names)<p>
    <p>
第7種：.data。到darknet的data資料夾中下載voc.data，然後修改裡面的內容。第一行的'classes = 20'數量修改成要訓練的類別數量、第四行的'names = data/voc.names'修改成'names = data/test.names'(test這個名稱是在上一步驟的.names檔的名稱，所以不一定每個人都要一樣)，存成自己想要的檔名(這次存成test.data)後放到./darkflow-master/cfg資料夾中。<p>
    
<h2>使用darkflow</h2> 
darkflow所有的使用方法可以直接參考darkflow(https://github.com/thtrieu/darkflow)網頁，只是要注意一點，在有用到flow這個功能的時候，如果跟我一樣是在命令提示字元下使用的，就要改成'python ./flow'。在這裡只記錄安裝及訓練這2個動作的流程(都在cmd下進行下列動作)<p>
<p>
1. 安裝：<p>  
    d:
    cd D:/darkflow-master
    python3 setup.py build_ext --inplace
    <p>
2. 查看flow有哪些功能:<p>
    python ./flow --h<p>
        <p>
    

<h2>reading recommandation</h2>  
http://christopher5106.github.io/object/detectors/2017/08/10/bounding-box-object-detectors-understanding-yolo.html
