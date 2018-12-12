import cv2
import time
import numpy as np
import math
import argparse

# set up time variable & get user input with options
start_time = time.time()
parser = argparse.ArgumentParser()
parser.add_argument('-i', help='input image')
parser.add_argument('-w', help='watermark image')
parser.add_argument('-o', help='output file name', default='overlaid.png')
opt = parser.parse_args()

# load images
background = cv2.imread(opt.i)
watermark = cv2.imread(opt.w)
output_path = opt.o

background = cv2.resize(background, (400, 200), interpolation=cv2.INTER_CUBIC)
watermark = cv2.resize(watermark, (300, 10), interpolation=cv2.INTER_CUBIC)

# initial variable setup
backG_height = background.shape[0]
backG_width = background.shape[1]
waterM_height = watermark.shape[0]
waterM_width = watermark.shape[1]

WHratio = min(float(waterM_height) / float(waterM_width), float(waterM_width) / float(waterM_height))
total_maxArea = maxAngle = maxRatio = 0
maxWM = watermark
watermark_area = waterM_width * waterM_height
alpha = 0.5

# To Boost!
def pre_resize_max_to_backG(image):
    if max(image.shape[0],image.shape[1]) == image.shape[1]:
        new_height = int(float(image.shape[0] * (float(backG_width) / float(image.shape[1]))))
        image = cv2.resize(image, (backG_width, new_height), interpolation=cv2.INTER_CUBIC)
    else:
        new_width = int(float(image.shape[1] * (float(backG_height) / float(image.shape[0]))))
        image = cv2.resize(image, (new_width, backG_height), interpolation=cv2.INTER_CUBIC)
    return image

# Affine transformation & Rotation with matrix
def myRotate(image, angle):
    # grab the dimensions of the image and then get the center coordinates
    (h, w) = image.shape[:2]
    (cenX, cenY) = (w // 2, h // 2)

    # grab the affine matrix for rotation (applying the angle to rotate counter-clockwise),
    # then grab the sine and cosine of rotation
    affine_M = cv2.getRotationMatrix2D((cenX, cenY), angle, 1.0)
    cos = np.abs(affine_M[0, 0])
    sin = np.abs(affine_M[0, 1])

    # compute the new rectangle boundary dimensions of the image
    new_W = int((h * sin) + (w * cos))
    new_H = int((h * cos) + (w * sin))

    # adjust the rotation matrix to take into account translation
    affine_M[0, 2] += (new_W / 2) - cenX
    affine_M[1, 2] += (new_H / 2) - cenY

    # perform the actual rotation and return the image
    return cv2.warpAffine(image, affine_M, (new_W, new_H))

# Main iteration
for angle in range(0,46):
    # else:
    ratio = temp_maxArea = 0
    enlarged_img = pre_resize_max_to_backG(watermark)
    #enlarged_img = watermark
    new_height = enlarged_img.shape[0]
    new_width = enlarged_img.shape[1]
    while(1):
        if new_height > new_width:
            new_height += ratio
            new_width = int(round(float(new_height) * float(WHratio)))
        else:
            new_width += ratio
            new_height = int(round(float(new_width) * float(WHratio)))
        enlarged_img = cv2.resize(enlarged_img, (new_width, new_height), interpolation=cv2.INTER_CUBIC)
        rot_img = myRotate(enlarged_img, angle)
        if rot_img.shape[1] <= backG_width and rot_img.shape[0] <= backG_height:
            ratio = 1
            if temp_maxArea < new_width * new_height:
                temp_maxArea = new_width * new_height
                if total_maxArea < temp_maxArea:
                    maxAngle = angle
                    total_maxArea = temp_maxArea
                    maxWM = rot_img
            if backG_height == rot_img.shape[0] or backG_width == rot_img.shape[1]:
                cv2.imshow("MAXIMUM AREA WATERMARK FOR EACH ANGLE", rot_img)
                cv2.waitKey(1)
                print(rot_img.shape[1], rot_img.shape[0], temp_maxArea)
                break
        else:
            ratio = -1
        print(rot_img.shape[1], rot_img.shape[0])

# make blank image with the same size as background image
tmp= np.zeros((backG_height,backG_width,3),np.uint8)

# cover mask with watermark image
top = backG_height // 2 - maxWM.shape[0] // 2
left = backG_width // 2 - maxWM.shape[1] // 2
tmp[top:top+maxWM.shape[0],left:left+maxWM.shape[1]] = maxWM

# overlay watermark on the input background
beta = (1.0 - alpha)
dst = cv2.addWeighted(background, alpha, tmp, beta, 0.0)

print("max Angle : " + repr(maxAngle))
elapsed_time = time.time() - start_time
print("Elapsed time : %s"%elapsed_time)

# save image
cv2.imwrite(output_path,dst)
cv2.imshow("destination",dst)
