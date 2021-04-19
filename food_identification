import numpy as np
import matplotlib.pyplot as plt
from skimage import io
from skimage.color import rgb2hsv

image = io.ImageCollection('C:/Users/Liza/Downloads/UNIMIB2016-images/original/*.jpg')


count_R_1 = 0
count_R_2 = 0
sum_mean_hue = {}
sum_mean_hue_test = {}


def saturation_choice(img_rgb):
    saturation_img = img_rgb[:, :, 1]

    for i in range(0, len(saturation_img)):
        for j in range(0, len(saturation_img[i])):
            if saturation_img[i][j] < 0.4:
                img_rgb[i][j] = 0

    return img_rgb

def sum_hue(img_rgb):

    saturation_img_01 = img_rgb[:, :, 1]
    hue_value = img_rgb[:, :, 0]
    count = 0
    sum_hue = 0
    product_table = []
    product_table_00 = []

    for ii in range(0, saturation_img_01.shape[0]):
        for jj in range(0, saturation_img_01.shape[1]):
            if (saturation_img_01[ii][jj] > 0):
                sum_hue += hue_value[ii][jj]
                count += 1
        if (count > 50):
            product_table.append(sum_hue / count)
            product_table_00.append(img_rgb[ii][:][:])
            count = 0
            sum_hue = 0

    return product_table,product_table_00

def segmentetion_s(sum_mean_hue,size1, size2,count_R_1):

    for x in range(size1, size2):
        product_oi = {}
        product_table_01 = []

        picture1 = image[x]
        img_rgb = rgb2hsv(picture1)
        img_rgb = saturation_choice(img_rgb)
        product_table, product_table_00 = sum_hue(img_rgb)

        count_88 = 0
        sum_mean_sum = 0
        count_R = 0

        for iii in range(0, len(product_table) - 1):
            if abs(product_table[iii] - product_table[iii + 1]) < 0.01:
                product_table_01.append(product_table_00[iii][:][:])
                count_88 += 1
            else:
                if (count_88 > 300):
                    product_oi[count_R] = product_table_01
                    count_R += 1
                    for i_i in range(len(product_table_01)):
                        for j_j in range(len(product_table_01[i_i])):
                            sum_mean_sum += product_table_01[i_i][j_j]
                    if (len(product_table_01) > 0):
                        sum_mean_hue[count_R_1] = sum_mean_sum / len(product_table_01)
                        count_R_1 += 1
                        sum_mean_sum = 0
                        product_table_01 = []
                count_88 = 0

    return sum_mean_hue


sum_mean_hue_train = segmentetion_s(sum_mean_hue,0, 10,count_R_1)
sum_mean_hue_test = segmentetion_s(sum_mean_hue_test,10, 11,count_R_2)


result = []

for i in range(len(sum_mean_hue_train)):
    for j in range(len(sum_mean_hue_test)):
        sum_result_0 = abs((sum_mean_hue_test[j][0]/sum_mean_hue_train[i][0])-\
                    (sum_mean_hue_test[j][1]/sum_mean_hue_train[i][1]))
        sum_result_1 = abs((sum_mean_hue_test[j][0] / sum_mean_hue_train[i][0]) - \
                       (sum_mean_hue_test[j][2] / sum_mean_hue_train[i][2]))
        sum_result_2 = abs((sum_mean_hue_test[j][1] / sum_mean_hue_train[i][1]) - \
                       (sum_mean_hue_test[j][2] / sum_mean_hue_train[i][2]))
        result.append(abs(1-(sum_result_0+sum_result_1+sum_result_2)/3))



    etalon_res = [0,3,4,7,11,13,17,18,19]
    plt.plot((result))
    plt.xticks(np.arange(0, 20,1))
    plt.plot([0,3,4,7,11,13,17,18,19],[1,1,1,1,1,1,1,1,1],"x")
    plt.title("Оценка соотношения параметров HSV теста и трен. выборки")
    plt.xlabel('Порядковый номер полученных сегментов')
    plt.ylabel('Оценка')
    plt.legend(loc='best')
    plt.show()
