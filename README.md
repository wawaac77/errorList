# errorList
***复盘清单（经验集）***    
    ```不要怕犯错，不轻易犯错```      
    ```能改变的努力改进，不能改变的坦然面对```

**1. Banner click所指向的登录页面，应该是旧版，却指向了还在UAT阶段的新版**   
   (_2018-8-16_)
   1. 在整个project都是新版login的branch中，我通过搜索loginViewController这个keyword，将全部位置替换成旧版login，做漏了一个位置。替换过程中，我有a.计算改过位置+不需更改位置，是否等于keyword搜索结果的总数；b.最后搜索loginViewController看是否有漏网之鱼。结果还是遗漏了一个位置，说明开发完再试下app的每个位置也是很重要的。
   
   2. 多人maintain同一个project时，如果有人使用了我的branch，要确保已经是自己检查过的，最好不要使用开发一半的。   


**2. 新版login网页的url，在release版本中，应该指向Production，却指向了UAT**   
   (_2018-8-22 my responsibility: code review_)
   1. 此url经过middleware再redirect去另外的url，这个新加的api，我应该在release之前再重点检查一次。因为缺少怀疑精神，和一点懒惰，没有去重新检查，导致没有发现domain被hard code的错误。  
   
   2. 这种UAT和Production用不同domain的情况，应该将其group到同一个位置，所有url的domain都从同一个位置控制，减少漏查的情况。  
   
   3. UAT和Production的控制，以及其他容错率低的位置，因iOS审批时间的原因，最好不要hard code，最好可以从backend控制，是其更灵活。   
     
    
**3. 新版login替换旧版login时，直接删除了代码中新版login web page没有返回的一个parameter，导致scan那部分功能因为有parameter是Null而crash**    
   (_2018-8-23 my responsibility: code review_)
  
   1. 删除parameter前要search整个project看是否影响其他地方。  
   
**4. Xcode “Cannot parse contents of Info.plist”** 
   (_2018-8-28_)
   1. To know which line is wrong: go the directory where the plist file is present, then write this command on terminal-> plutil filename.plist
   2. Because I didn's select build no when merge. Then info.plist kept both and caused parsing error
   
