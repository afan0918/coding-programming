# coding-programming

工學院專題競賽資料，拿來放成果的repository

真的好快樂喔哈哈哈哈

## 集群運動

### PSO演算法(粒子群演算法)

粒子群演算法是為了模擬鳥類飛行行為所發展出來的演算法，在粒子群演算法中一個粒子代表鳥群中的一隻鳥，各個粒子具有記憶性並會參考其它粒子的訊息來決定移動的方向。

簡單來說，一群被稱為粒子的潛在解在多維度解空間中找尋最佳解位置，粒子每次移動都會參考自身過往曾找到的的最佳解位置、所有例子的過往最佳解位置，然後再決定移動方向還有距離。

白話來說的意思為 : *粒子目前移動速度 = 粒子上個時間的移動速度 + 自己認為的最佳方向 + 群體認為的最佳方向*，透過不斷移動一群粒子來在有限時間內得到較好的解。

目前主流用途與基因演算法類似，被拿來作為解決最佳化問題的工具，因為這類問題大多都可能是NP問題，不過我想做的事情是將粒子的行為可視化，因此將能求解的問題維度降低到2維上。

程式碼另外放在這個[repository](https://github.com/afan0918/PSO)裡面，模擬加上畫面以及註解呈現的程式碼大概只需要三百行左右，想要更改粒子行為的話在[PSO.java](https://github.com/afan0918/PSO/blob/main/PSO.java)中更改此方程即可。

```java
    /**
     * 要求解的方程式
     * @param x 輸入粒子位置
     * @return 粒子適應程度
     */
    public double func(double[] x) {
        if (x[0] == 0 && x[1] == 0) {
            return Math.exp((Math.cos(2 * Math.PI * x[0]) + Math.cos(2 * Math.PI * x[1])) / 2);
        } else {
            return Math.sin(Math.sqrt(Math.pow(x[0], 2) +
                    Math.pow(x[1], 2))) / Math.sqrt(Math.pow(x[0], 2) +
                    Math.pow(x[1], 2)) + Math.exp((Math.cos(2 * Math.PI * x[0]) +
                    Math.cos(2 * Math.PI * x[1])) / 2);
        }
    }
```

#### 成果展示

https://user-images.githubusercontent.com/70462625/154410869-95f9d299-f1c9-40ed-8926-ed02fc0c1ae7.mp4

## 剛體運動

使用[Rigid_Bunny.cs](https://github.com/afan0918/coding-programming/blob/main/%E5%89%9B%E9%AB%94%E9%81%8B%E5%8B%95/Rigid_Bunny.cs)腳本在unity中運行的結果。

實現基礎的碰撞檢測與旋轉角速度計算。

https://user-images.githubusercontent.com/70462625/163896326-ce98145e-4958-45c8-9224-d3ec0fdd9313.mp4

使用[Rigid_Bunny_by_Shape_Matching.cs](https://github.com/afan0918/coding-programming/blob/main/%E5%89%9B%E9%AB%94%E9%81%8B%E5%8B%95/Rigid_Bunny_by_Shape_Matching.cs)腳本在unity中運行的結果。

從影片中可以明顯看出來這樣實現的兔子看起來會比較軟一點，陷進牆內的部分越多，獲得的反方向加速度越高，缺點是體積較小的部分容易有明顯的下陷，像是影片中就能夠明顯的看到兔子尾巴和耳朵陷入地板的部分。

https://user-images.githubusercontent.com/70462625/163896317-5d62b992-b25e-451e-baa2-9cd44eee4fa7.mp4

## 布料模擬

實時布料模擬目前主流方法有兩種，一種是基於隱式積分法進行模擬，一種是基於PBD，隱式積分法我還沒有念到很懂，可能之後實現。

### Position Based Dynamics (PBD)

程式碼主體在[PBD_model.cs](https://github.com/afan0918/coding-programming/blob/main/%E5%B8%83%E6%96%99%E6%A8%A1%E6%93%AC/PBD_model.cs)

#### 成果展示

![image](https://user-images.githubusercontent.com/70462625/163804059-4b5ee5f2-f709-4d9b-b7d7-7e0e8995e450.png)

https://user-images.githubusercontent.com/70462625/163911263-5ae534f1-698c-4296-8a7c-fa01f07ecfe2.mp4

## 彈性體模擬

彈性體我覺得是很難的領域，這部分就跟材料力學比較有關係，程式碼寫起來就在計算 Ic 和 IIc，主流的幾種彈性體模擬方法可以看以下的影片展示。

https://user-images.githubusercontent.com/70462625/163926922-3ba03331-1273-42a1-812b-2be85fc28215.mp4

Descent Methods for Elastic Body Simulation on the GPU (SIGGRAPH Asia 2016)

實現我採用最簡單的StVK方法，打算等流體寫完之後再來挑戰看看 Neo-Hookean，畢竟 StVK 雖然方便，但缺點真的太大了，可從下圖看到 StVK 的抵抗力在過了閥值之後，反而會減小，甚至會變成負的導致模型崩潰，實際跑的過程中不能夠給模型施加太大的力，有點麻煩。

主要程式碼在[FVM.cs](https://github.com/afan0918/coding-programming/blob/main/%E5%BD%88%E6%80%A7%E9%AB%94%E6%A8%A1%E6%93%AC/FVM.cs)，採用的方法是有限體積的做法進行模擬。

![image](https://user-images.githubusercontent.com/70462625/163927284-c2b2f0e3-3872-4dd2-87f6-91a3be23a287.png)
Irving et al. 2004. Invertible Finite Elements For Robust Simulation of Large Deformation. SCA

![image](https://user-images.githubusercontent.com/70462625/166108859-947a3200-9943-48cf-8da0-70616f2649af.png)

(後續補充:其實按照這個做，把程式碼內的StVK方法替換掉就好了，其他部分都不用管)

#### 成果展示

https://user-images.githubusercontent.com/70462625/164390281-169a18df-ab4b-4d5e-8f61-baf65a5d5e4d.mp4

## 流體模擬

過程中有點像下雨的部分是加水的過程(讓水位增高)，程式碼主體在[wave_motion.cs](https://github.com/afan0918/coding-programming/blob/main/%E6%B0%B4%E9%AB%94%E6%A8%A1%E6%93%AC/wave_motion.cs)

#### 成果展示

https://user-images.githubusercontent.com/70462625/164892352-3cbe577e-77d3-476d-b17a-dffa8d5f997e.mp4

### 參考資料與模型來源

* Games103 : 基於物理的計算機動畫入門，王華民(俄亥俄州立大學)
* Games202 : 高質量實時渲染，閆令琪(加州大學聖塔芭芭拉分校)
* 剛體模擬相關論文 :
1. Muller et al. 2005. Meshless Deformations Based on Shape Matching. TOG (SIGGRAPH).
* 布料模擬相關論文 :
2. Baraff and Witkin. 1998. Large Step in Cloth Simulation. SIGGRAPH.
3. Bridson et al. 2003. Simulation of Clothing with Folds and Wrinkles. SCA.
4. Bergou et al. 2006. A Quadratic Bending Model for Inextensible Surfaces. SCA.
5. English and Bridson. 2008. Animating Developable Surfaces Using Nonconforming Elements.
SIGGRAPH. (optional)
* 彈性體模擬相關論文 :
6. Irving et al. 2004. Invertible Finite Elements For Robust Simulation of Large Deformation.
SCA
7. Wang. 2016. Descent Methods for Elastic Body Simulation on the GPU. TOG (SIGGRAPH
Asia).
8. Xu et al. 2015. Nonlinear Material Design Using Principal Stretches. TOG (SIGGRAPH).
* 流體模擬相關論文 :
9. Kass and Miller. 1990. Rapid, Stable Fluid Dynamics for Computer Graphics. Computer
Graphics.
10. Jos Stam. 1999. Stable Fluids. TOG (SIGGRAPH).

### 正在實現的SPH

念完SPH了，好像還行，先把筆記丟上來好了，之後實作出來再撤掉。

![image](https://user-images.githubusercontent.com/70462625/165312529-7e571d12-2dfd-4b12-9e3c-f7be763c7fb3.png)
![image](https://user-images.githubusercontent.com/70462625/165312571-a993461e-3203-415d-a1c5-b1a4b5dbebc8.png)

實作計算上對粒子位置進行迭代的計算順序:

![image](https://user-images.githubusercontent.com/70462625/165312916-11e0a333-5f4d-4ec7-9d1d-dfc4f132a8fb.png)

不過粒子最終還要把他轉換成三角形，才能透過顯卡進行渲染，這部分可能就不做了，知識盲區
![image](https://user-images.githubusercontent.com/70462625/165313651-212dc501-33ea-4850-a0cd-236f45f2f5df.png)





