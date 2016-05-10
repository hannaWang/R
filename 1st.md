##R intro
Functional Programming
```{r, echo=TRUE}
x=1:5
sum(x) 
mean(x)
var(x)
sqrt(var(x))
sd(x)
```
Object Oriented Language
- http://rightthewaygeek.blogspot.tw/2013/10/r-oop1-class.html
- http://blog.fens.me/r-class-s3/
- http://stackoverflow.com/questions/27219132/creating-classes-in-r-s3-s4-r5-rc-or-r6
```{r}
#S4 class example
setClass("BMI",
  representation(
    name = "character",
    weight = "numeric",
    height = "numeric"
  )         
)
setMethod(f='show',"BMI",function(obj){
            BMI <-obj@weight/((obj@height/100)^2)
            cat(obj@name,"\n")
            cat("BMI= ",BMI)
          }
)
john <- new("BMI",name="JOHN",weight=70,height=182)

```


## RDemo
```{r RDemo, echo=TRUE}
install.packages("car")
library("car")
data(Quartet)

plot(Quartet$x, Quartet$y1)
fit = lm(Quartet$y1 ~ Quartet$x)
abline(fit, col="red")
```


##R basic command
```{R}
#文件查詢
help(package="base")
?base:sum
?sum
help.search("sum")

demo()
data()
ls()
rm()
typeof()
class()
str()
```

##Basic computing
```{R, echo=TRUE}
3+8
3-8
3*8
3/8
1/0
11%%2
```

##Assignment
```{R}
a = 3
a <- 5
fit = lm(y1 ~ x, data =Quartet)
fit <- lm(y1 ~ x, data =Quartet)
```

##Basic type
- num: 1,2,1.2
- integer: 1L,2L,3L
- chracter: "string"
- logical: TRUE,FALSE
- complex: 1+4i

## Basic Objects
atomic:
(由相同資料型態組成)
- vector
- matrix
- factor
recursive:
(可以有混合的資料型態)
- dataframe
- list

##Vector
- R語言最基本的物件
```{R, echo=TRUE}
character(5)  ## character vector of length 5
numeric(5)
logical(5)
x = c(1,2,3,7)
y= c(2,3,5,1)
x+y
x*y
x - y
x/y

x + 10
x + c(10)
x + c(1,2)
x + c(1,2,1,2)

x = c(1,2,3,4,NA)
sum(x)
sum(x, na.rm=T)

height_vec = c(180,169,173)
height_vec
names(height_vec) = c("Brian", "Toby", "Sherry")
height_vec

name_vec = c("Brian", "Toby", "Sherry")
names(height_vec) = name_vec
height_vec > 175
height_vec / 100
height_vec > 175 | height_vec < 170
height_vec < 175 & height_vec > 170

```
###p42 example
```{R, echo=TRUE}
h = c(180,169,173)
w = c(73,87,43)
bmi = w / ((h/100)^2)
names(bmi) = c("Brian", "Toby", "Sherry")
bmi < 18.5 | bmi >= 24
```

##seq() & rep() & paste()
```{R, echo=TRUE}
1:20
seq(1,20)
seq(1,20,2)
seq(1,20,by=2)
seq(1,20,length=4)  #切成4等分

rep(1,5)  #rep(值,次數)
rep(c(1,2), times=5)
rep(c(1,2), times=c(1,2)) #times=c(1,2):c(1, )一次,c( ,2)二次
rep(c(1,2), each=5)
rep_len(c(1,2),5) #rep c(1,2),長度為5

#paste converts its arguments (via as.character ) to character strings, and concatenates them
paste("the","big","bang","theory")
paste("big","bang",sep="")
paste("big","bang",sep=";")
paste(c("big","bang"),1:4)  # "big 1"  "bang 2" "big 3"  "bang 4"
paste(c("big","bang"),1:4,collapse = "+" )
```

##Matrix
```{R}
matrix(1:9, byrow=TRUE, nrow=3)
matrix(1:9, nrow=3)
kevin = c(85,73)
marry = c(72,64)
jerry = c(59,66)
mat = matrix(c(kevin, marry, jerry), nrow=3, byrow= TRUE)
colnames(mat) = c('first', 'second')
rownames(mat) = c('kevin', 'marry', 'jerry')
mat

# basic
dim(mat)
nrow(mat)
ncol(mat)
t(mat) #transpose
mat[1,]
mat[,1]
mat[1:2,]
rowSums(mat)
colSums(mat)

# insert new value
mat2 = rbind(mat, c(78,63))
rownames(mat2)[nrow(mat2)] = 'sam'
mat2

mat3 = cbind(mat2,c(82,77,70,64))
colnames(mat3)[ncol(mat3)] = 'third'
mat3

rowMeans(mat3)
colMeans(mat3)


# arithmetic
m1 = matrix(1:4, byrow=TRUE, nrow=2)
m2 = matrix(5:8, byrow=TRUE, nrow=2)

m1 + m2
m1 - m2
m1 * m2
m1 / m2

m1 %*% m2

```

###p53 example
```{R}
kevin = c(85,73)
marry = c(72,64)
jerry = c(59,66)
mat = matrix(c(kevin, marry, jerry), nrow=3, byrow= TRUE)
colnames(mat) = c('first', 'second')
rownames(mat) = c('kevin', 'marry', 'jerry')

final = mat %*% c(0.4,0.6)
final
```

##Factor
```{R}
# syntax
weather= c("sunny","rainy", "cloudy", "rainy", "cloudy")
class(weather)
weather_category = factor(weather)
weather_category
class(weather_category)
# order
temperature = c("Low", "High", "High", "Medium", "Low", "Medium")
temperature_category = factor(temperature, order = TRUE, levels = c("Low", "Medium", "High"))
temperature_category
temperature_category[3] > temperature_category[1]
temperature_category[4] > temperature_category[3]

# change levels name
weather= c("s","r", "c", "r", "c")
weather_factor = factor(weather)    #向量轉換為類別才能提取level
levels(weather_factor)    #(c,r,s))
levels(weather_factor) = c("cloudy","rainy","sunny")  #與levels(weather_factor)位置對應並替換
weather_factor
```

##Dataframe
```{R}
df = data.frame(a=c(1,2,3,4,5),b=c(2,3,4,5,6))
df
class(df)
str(df)

data(iris)
head(iris, 10)
head(iris)
tail(iris, 10)
tail(iris)
str(iris)    #檢視資料架構

iris[1:3,]
iris[1:3,1]
iris[1:3,"Sepal.Length"]  #Sepal.Length為columnname(變數名)
head(iris[,1:2])
iris$"Sepal.Length" #列出變數:Sepal.Length的所有值

Five.Sepal.iris = iris[1:5, c("Sepal.Length","Sepal.Width")]  #取前五筆包含length 及 width 的資料
setosa.data = iris[iris$Species=="setosa",1:5]  #用條件做篩選:取變數Species,值為setosa
str(setosa.data)   #檢視資料架構

#使用which 做資料篩選(僅列出標題列)
which(iris$Species=="setosa") 

#attach() detach()
#注意attach只是目前資料集的快照(snapshot),若原始資料有更動,attach中的資料不會跟著變動
attach(iris)
Species
setosa.data = iris[Species=="setosa",1:5]
iris[1,1]
iris[1,1] = 5.5 #更換dataframe裡面的資料值
iris
Sepal.Length[1] #快照(snapshot),原始資料更動attach中的資料卻不動
detach()

#merge進行資料合併
flower.type = data.frame(Species = "setosa", Flower = "iris")    #建立dataframe, 並給變數與變數值
merge(flower.type, iris[1:3,], by ="Species")    #使用兩表都有的變數:Species合併

#用order做資料排序
iris[order(iris$Sepal.Length, decreasing = TRUE),]

#繪圖
table.iris = table(iris$Species)
pie(table.iris)
hist(iris$Sepal.Length)    #長條圖
boxplot(Petal.Width ~ Species, data = iris)
plot(x=iris$Petal.Length, y=iris$Petal.Width, col=iris$Species)

##File read and write
```{R}
getwd()    #取得目前工作區
setwd("C:/Users/BigData/Desktop")    #更換預設路徑
tw2330 = read.csv("table.csv", header=TRUE)    #讀取檔案
test.data = read.table("match.txt" ,header = TRUE, sep="|") #header = TRUE:表頭為變數名稱

write.table(test.data, file = "test.txt" , sep = " ") #寫入table
write.csv(test.data, file = "test.csv") #寫入csv

```

###p68 example
```{R}
tw2330 = read.csv("Class/data/2330.csv", header = TRUE)
stock = tw2330[(tw2330$Date >= '2015-03-01' & tw2330$Date < '2015-08-31'),]  #用條件篩選做資料選取,[參數1:選列,參數2:選行]
str(tw2330) #檢視變數型態
tw2330$Date = as.Date(tw2330$Date)  #變數轉型(factor->date):as.Date
max(tw2330$Close)

summary(stock$Close)
hist(stock$Close) #Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
boxplot(stock$Close)
plot(stock$Date, stock$Close)

ordered_stock = stock[order(stock$Close, decreasing = T),]
ordered_stock[1,]
ordered_stock[nrow(ordered_stock),]
ordered_stock[1,"Close"] - ordered_stock[nrow(ordered_stock),"Close"]

```

##List
```{R}
item = list(thing="hat", size="8.25")
item

test = list(name="Toby", score = c(87,57,72))
test$score
test$score[2]

flower= list(title="iris dataset", data= iris)
class(flower)
class(flower$data)
flower$data[1,"Sepal.Width"]
```

##Flow Contorl
```{R}
x=5;
if(x>3){
  print
  ("x > 3");
}else{
  print
  ("x <= 3");
}


x=5;
if(x>3){
  print ("x > 3");
} else if (x ==3){
  print ("x == 3");
}else{
  print
  ("x <= 3");
}

x=5;
if(x>5){
  print ("aaa");
} else if (x <= 5 & x > 3 ){
  print ("bbb");
}else{
  print
  ("ccc");
}


for(i in 1:10){
  print(i);
}

sum=0
for(i in 1:100){
  sum= sum+ i;
}
sum

sum(1:100)


ary = rep(NA, 100)
for(i in 1:100){
  ary[i]= i;
}
ary


ary2 =c()
for(i in 1:100){
  ary2 = c(ary2, i);
}
ary2

sum = 0;
cnt = 0;
while(cnt <= 100){
  sum = sum + cnt;
  cnt = cnt + 1;
}
sum
```

###page81 example
```{R}
nine_nine = function(){
  mat = matrix(rep(1,9^2),nrow = 9)
  for(i in 1:9){
    for(j in 1:9){
      mat[i,j] = i * j;
    }
  }
  mat
}
nine_nine()  #欲印出矩陣需執行函數

nine_nine2 = function(){
  mat1 = matrix(1:9, nrow = 9);
  mat2 = matrix(1:9, nrow = 1);
  mat = mat1 %*% mat2;
  mat
}
nine_nine2()  #欲印出矩陣需執行函數
```

##Function
```{R}
f = function(a){
    print(a+3)
}

f1 = function(a = 3) {
    print(a+3)
}

f2 = function(a, b = 2, c = NULL) {
   a + b
}

b = 3
f3 = function(a) {
    b = 2
    b
}

f4 = function(a,b){
    if(a > 3){
       a = 100;
    }
    a + b;
}
```

###p84  example
```{R}
match_func = function(filename= "data/match.txt"){
  mat = matrix(-1, nrow=5, ncol = 5)
  rownames(mat) = c("A","B","C","D","E")
  colnames(mat) = c("A","B","C","D","E")
  
  match = read.table(filename, sep= "|")
  for (i in 1:nrow(match)){ #nrow():The Number of Rows/Columns of an Array
    mat[match[i,1], match[i,2]] = match[i,3];
  }
  mat
}
match_func("Class/data/match.txt")

###p84-2
match_func = function(filename){  #filename is parameter
  t = read.table(filename,sep = '|');
  c = 0;
  mat = matrix(rep(-1,length(levels(t[,1]))^2), nrow = length(levels(t[,1])), dimnames = list(levels(t[,1]),levels(t[,2])));
  for(i in seq(nrow(mat))){
    for(j in seq(ncol(mat))){
      if(i != j){
        mat[i,j] = t[c+1,3];
        #print(t[c,3]);
        c = c + 1;
      }
    }
  }
  mat
}
match_func("Class/data/match.txt")
```

##lapply sapply apply tapply
```{R}
grades =list(kevin = c(80,60,92), marry = c(56,75,64,84,56), QOO = c(10,20,3,4,10))
unlist(grades[1]) #取出list:grades內的值轉成為向量
for (i in 1:length(grades)){  #i(自定義的變數) in l(list), 長度為list的長度
      print(sum(unlist(grades[i])));
}

lapply(grades, sum) 
lapply(grades, mean)  
lapply(grades, function(e){list(sum = sum(e), min = min(e))})

lapply(c(1,2,3,4,5),  function(e){paste(as.character(e), "hello")}) #paste:轉int為char, ",":append
m1 = matrix(1:4, byrow=TRUE, nrow=2)
m2 = matrix(5:8, byrow=TRUE, nrow=2)

li = list(m1, m2)
lapply(li, mean)


class(lapply(grades, sum))
sapply(grades, sum)

m1 = matrix(1:4, byrow=TRUE, nrow=2)
m2 = matrix(5:8, byrow=TRUE, nrow=2)
li = list(m1, m2)
sapply(li, mean)  #對list:li內m1,m2取mean
sapply(li,function(e) e[1,])  #對list:li內m1,m2執行function(e矩陣):對矩陣取第一列

m = matrix(1:4, byrow=TRUE, nrow=2)
apply(m, 1, sum) #for a matrix 1 indicates rows, 2 indicates columns, c(1, 2) indicates rows and columns
apply(m, 2, sum) #colsums

rowmeans = apply(m, 1, mean)
colmeans = apply(m, 2, mean)

x = c(80,70,59,88,72,57)
t = c(1,1,2,1,1,2)
tapply(x,t, mean) #x依t分組取mean

data(iris)
tapply(iris$Sepal.Length, iris$Species,mean)

```
