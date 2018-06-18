# darkflow

<b>code resouces : https://github.com/thtrieu/darkflow</b><p>
<b>labeling tool : https://github.com/tzutalin/labelImg</b>
<b>dataset preparation referece : https://blog.csdn.net/hysteric314/article/details/54097845</b>

<h3>提醒：這是將自己測試過程、遇到的困難都做一個紀錄</h3>

目標：用自己做的dataset，來訓練yolo/darkflow

準備dataset：必須要有7種東西，如下說明

第1種：要train的影像。我用的是.jpg，影像大小多少都不會影響（至少我的是這樣啦）。放在./darkflow-master/train/Images資料夾中

第2種：label.txt檔。裡面是類別名稱，一行寫1個類別名稱，所以如果要train yolo去分辨4種類別，在label.txt中就會有4行。放在./darkflow-master資料夾中

第3種：影像的label資料，必須是.xml檔，所以用labelImg這個工具，一開始就先選好最後是輸出.txt(選YOLO)還是.xml檔(選VOC)。輸出的.xml放在./darkflow-master/train/Annotations資料夾中

第4種：.cfg

第5種：.weight

第6種：.data

第7種：.names


reading recommandation : http://christopher5106.github.io/object/detectors/2017/08/10/bounding-box-object-detectors-understanding-yolo.html
