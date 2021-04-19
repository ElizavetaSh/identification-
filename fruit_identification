from skimage import filters
import numpy as np
import matplotlib.pyplot as plt
from skimage import io
from skimage.color import rgb2gray
from skimage.measure import find_contours

image = io.ImageCollection('F:/Documents/Fight/CucumberRipe_2/*.jpg')
testind_image = io.ImageCollection('F:/Documents/Fight/CucumberRipe_2_testing/*.jpg')

def dlina_shirina(image):

    data_pic_1 = []
    data_pic_2 = []

    data_pic_max_x = []
    data_pic_min_x = []

    for i in range(0,1):
        picture1 = image[i]
        picture2 = rgb2gray(picture1)
        picture3 = filters.sobel(picture2)
        picture_01 = find_contours(picture3,0.1)

        fig, (ax1, ax2, ax3) = plt.subplots(ncols=3, figsize=(8, 4),
                                            sharex=True, sharey=True)

        ax1.imshow(picture1)
        ax2.imshow(picture2,cmap='gist_gray')
        ax3.imshow(picture3,cmap='gist_gray')
        plt.show()
        for j in range(0,len(picture_01)-1):
            for i in range(0,len(picture_01[j])):
                data_pic_1.append(picture_01[j][i][0])
                data_pic_2.append(picture_01[j][i][1])


        data_pic_1_max= max(data_pic_1)
        data_pic_1_min= min(data_pic_1)
        data_pic_2_max= max(data_pic_2)
        data_pic_2_min= min(data_pic_2)
        data_pic_1 = []
        data_pic_2 = []
        y_max = 0
        y_min=0
        x_max =0
        x_min =0
        for j in range(0,len(picture_01)-1):
            for i in range(0,len(picture_01[j])):
                if (abs(picture_01[j][i][0] -data_pic_1_max) < 0.005):
                    y_max = picture_01[j][i][0]

                if (abs(picture_01[j][i][0] - data_pic_1_min)< 0.005):
                    y_min = picture_01[j][i][0]

                if (abs(picture_01[j][i][1] - data_pic_2_max)< 0.005):
                    x_max = picture_01[j][i][1]

                if (abs(picture_01[j][i][1] - data_pic_2_min) < 0.005):
                    x_min = picture_01[j][i][1]
        data_pic_max_x.append(((data_pic_1_max - data_pic_1_min) ** 2 + (y_max - y_min) ** 2) ** (1 / 2))
        data_pic_min_x.append(((data_pic_2_max - data_pic_2_min) ** 2 + (x_max - x_min) ** 2) ** (1 / 2))
    return (data_pic_max_x,data_pic_min_x)



data_pic_max_x,data_pic_min_x = dlina_shirina(image)
data_pic_max_x_test,data_pic_min_x_test= dlina_shirina(testind_image)
print(data_pic_max_x)
print(data_pic_max_x_test)
print(data_pic_min_x)
print(data_pic_min_x_test)
result = []
count = 0
porog_value = 0.08
for j in range(0,len(data_pic_max_x_test)):
    for i in range(0, len(data_pic_max_x)):
        if (abs(data_pic_max_x[i]-data_pic_max_x_test[j]) < porog_value)&\
                (abs(data_pic_min_x[i]-data_pic_min_x_test[j]) < porog_value):
            count =count+1
    if count>0.5:
        result.append(1)
    else:
        result.append(0)
    count=0


result_etalon = np.zeros(297)
for i in range(0,len(result_etalon)):
    if (75<i<153)|(160<i<166)|(168<i<169)|(171<i<172)|(174<i<186)|(193<i<194)|(196<i<241)|(i == 278)|(289<i<293):
        result_etalon[i] = 1


error_fun = []
count_01 = 0
new_fun  = []
new_fun_x  = []
print (len(result_etalon))
print (len(result))
for i in range(0,len(result_etalon)):
    if (result_etalon[i]==1)&(result[i]==0):
        error_fun.append(1)
        count_01=count_01+1
    elif (result_etalon[i]==0)&(result[i]==1):
        error_fun.append(-1)
        count_01 =count_01 +1
    else:
        error_fun.append(0)
    if (result_etalon[i]!= result[i]):
        new_fun.append(1)
        new_fun_x.append(i)


print(count_01)
print("Процентное соотношение: ",round((len(result_etalon)-count_01)/len(result_etalon),3))


plt.figure(figsize=(6,5))
plt.hist(np.array(result_etalon)+0.05,color='r',label='Истинные значения')
plt.hist(np.array(result)-0.05,label='Полученные значения')
plt.axis([-0.2, 1.2, 0, 250])
plt.xticks(np.arange(-0.2, 1.2,0.2))
plt.title("Гистограмма результатов")
plt.xlabel('Значения функции распознования')
plt.ylabel('Количесво значений функции распознования')
plt.legend(loc='best')
plt.show()
