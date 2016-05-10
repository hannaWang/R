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


##seq() & rep() & paste()
```{R, echo=TRUE}
1:20
seq(1,20)
seq(1,20,2)
seq(1,20,by=2)
seq(1,20,length=2)

rep(1,5)
rep(c(1,2), times=5)
rep(c(1,2), times=c(1,2))
rep(c(1,2), each=5)
rep_len(c(1,2),5)

paste("the","big","bang","theory")
paste("big","bang",sep="")
paste("big","bang",sep=";")
paste(c("big","bang"),1:4)
paste(c("big","bang"),1:4,,collapse = "+" )
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

#取前五筆包含length 及 width 的資料
Five.Sepal.iris = iris[1:5, c("Sepal.Length","Sepal.Width")]
#可以用條件做篩選
setosa.data = iris[iris$Species=="setosa",1:5]  #選取變數Species,值為setosa
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

#找出最高收盤價
tw2330 = read.csv("Class/data/2330.csv", header = TRUE)
stock = tw2330[(tw2330$Date < "2015-05-14" & tw2330$Date > "2015-05-01"),]  #用條件篩選做資料選取,[參數1:選列,參數2:選行]
str(tw2330$Date)  #檢視變數型態
tw2330$Date = as.Date(tw2330$Date)  #變數轉型(factor->date):as.Date
str(tw2330)
max(stock$Close)

```
##File read and write
```{R}
getwd()    #取得目前工作區
setwd("C:/Users/BigData/Desktop")    #更換預設路徑
tw2330 = read.csv("table.csv", header=TRUE)    #讀取檔案
test.data = read.table("match.txt" ,header = TRUE, sep="|") #header = TRUE:表頭為變數名稱

#寫入table
write.table(test.data, file = "test.txt" , sep = " ")
#csv
write.csv(test.data, file = "test.csv")

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
##lapply sapply apply tapply
```{R}
grades =list(kevin = c(80,60,92), marry = c(56,75,64,84,56), QOO = c(10,20,3,4,10))

unlist(grades[1])
for (i in 1:length(grades)){
      print(sum(unlist(grades[i])));
}

lapply(grades, sum)
lapply(grades, mean)
lapply(grades, function(e){list(sum = sum(e), min = min(e))})

lapply(c(1,2,3,4,5),  function(e){paste(as.character(e), "hello")})
m1 = matrix(1:4, byrow=TRUE, nrow=2)
m2 = matrix(5:8, byrow=TRUE, nrow=2)

li = list(m1, m2)
lapply(li, mean)


class(lapply(grades, sum))
sapply(grades, sum)

m1 = matrix(1:4, byrow=TRUE, nrow=2)
m2 = matrix(5:8, byrow=TRUE, nrow=2)
li = list(m1, m2)
sapply(li, mean)
sapply(li,function(e) e[1,])

m = matrix(1:4, byrow=TRUE, nrow=2)
apply(m, 1, sum) # rowsums
apply(m, 2, sum) # colsums

rowmeans = apply(m, 1, mean)
colmeans = apply(m, 2, mean)

x = c(80,70,59,88,72,57)
t = c(1,1,2,1,1,2)
tapply(x,t, mean)

data(iris)

tapply(iris$Sepal.Length, iris$Species,mean)

```
