import numpy as np
import cv2
#create a numpy array to store the reference points
p=np.zeros((4,2),dtype='float32')
i=0
def order_points(pts):
	# initialzie a list of coordinates that will be ordered
	# such that the first entry in the list is the top-left,
	# the second entry is the top-right, the third is the
	# bottom-right, and the fourth is the bottom-left
	rect = np.zeros((4, 2), dtype = "float32")
 
	# the top-left point will have the smallest sum, whereas
	# the bottom-right point will have the largest sum
	s = pts.sum(axis = 1)
	rect[0] = pts[np.argmin(s)]
	rect[2] = pts[np.argmax(s)]
 
	# now, compute the difference between the points, the
	# top-right point will have the smallest difference,
	# whereas the bottom-left will have the largest difference
	diff = np.diff(pts, axis = 1)
	rect[1] = pts[np.argmin(diff)]
	rect[3] = pts[np.argmax(diff)]
 
	# return the ordered coordinates
	return rect
def four_point_transform(image, pts):
	
	# obtain a consistent order of the points and unpack them
	# individually
	rect = order_points(pts)
	(tl, tr, br, bl) = rect
 
	# compute the width of the new image, which will be the
	# maximum distance between bottom-right and bottom-left
	# x-coordiates or the top-right and top-left x-coordinates
	widthA = np.sqrt(((br[0] - bl[0]) ** 2) + ((br[1] - bl[1]) ** 2))
	widthB = np.sqrt(((tr[0] - tl[0]) ** 2) + ((tr[1] - tl[1]) ** 2))
	maxWidth = max(int(widthA), int(widthB))
 
	# compute the height of the new image, which will be the
	# maximum distance between the top-right and bottom-right
	# y-coordinates or the top-left and bottom-left y-coordinates
	heightA = np.sqrt(((tr[0] - br[0]) ** 2) + ((tr[1] - br[1]) ** 2))
	heightB = np.sqrt(((tl[0] - bl[0]) ** 2) + ((tl[1] - bl[1]) ** 2))
	maxHeight = max(int(heightA), int(heightB))
 
	# now that we have the dimensions of the new image, construct
	# the set of destination points to obtain a "birds eye view",
	# (i.e. top-down view) of the image, again specifying points
	# in the top-left, top-right, bottom-right, and bottom-left
	# order
	dst = np.array([
		[0, 0],
		[maxWidth - 1, 0],
		[maxWidth - 1, maxHeight - 1],
		[0, maxHeight - 1]], dtype = "float32")
 
	# compute the perspective transform matrix and then apply it
	M = cv2.getPerspectiveTransform(rect, dst)
	warped = cv2.warpPerspective(image, M, (maxWidth, maxHeight))
 
	# return the warped image
	return warped
def point_collector(event,x,y,flags,param):
	# a funtion to collect the points on clicking th left mouse button at that point
	global p,i
	if event==cv2.EVENT_LBUTTONDOWN:
		p[i]=[x,y]
		i=i+1
		cv2.circle(img,(x,y),3,(0,0,255),-1)
		#mark the points by making a small circle at the point
	cv2.imshow('image',img)
def iterator():
	global img,clone,i
	while True:
		cv2.imshow('image',img)
		k=cv2.waitKey(1)&0xFF
		if i==4:
	#break after the four points have been marked
			break
		elif k==ord('r'):
	#in order to reset, if we make NY MISTAKE WHILE CHOOSING TH POINTS
			i=0
			img=clone.copy()
		
img=cv2.imread('abc.jpg',1)
clone=img.copy()
cv2.namedWindow('image')
print("select the left image")
cv2.setMouseCallback('image',point_collector)
iterator()
img=clone.copy()
left = four_point_transform(img,p)
cv2.imshow("left", left)
print("select the center image")
img=clone.copy()
i=0
cv2.setMouseCallback('image',point_collector)
iterator()
img=clone.copy()
center= four_point_transform(img,p)
cv2.imshow("center", center)
print('select the right image')
img=clone.copy()
i=0
cv2.setMouseCallback('image',point_collector)
iterator()
img=clone.copy()
right = four_point_transform(img,p)
cv2.imshow("right", right)
(h,b)=center.shape[:2]
r=h/left.shape[0]
dim=(int(left.shape[1]*r),h)
releft=cv2.resize(left,dim,interpolation=cv2.INTER_AREA)
cv2.imshow('left',releft)
r=h/right.shape[0]
dim=(int(right.shape[1]*r),h)
reright=cv2.resize(right,dim,interpolation=cv2.INTER_AREA)
cv2.imshow('right',reright)
h1, w1 = releft.shape[:2]
h2, w2 = center.shape[:2]
h3, w3 = reright.shape[:2]
#create empty matrix
vis = np.zeros((max(h1, h2,h3), w1+w2+w3,3), np.uint8)

#combine 2 images
vis[:h1, :w1,:3] = releft
vis[:h2, w1:w1+w2,:3] = center
vis[:h3, w1+w2:w1+w2+w3, :3]=reright
cv2.imshow('grand finale',vis)
cv2.waitKey(0)
cv2.destroyAllWindows()



