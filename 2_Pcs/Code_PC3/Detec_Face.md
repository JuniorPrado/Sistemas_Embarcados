```C
#include<stdio.h>
#include<math.h>
#include<opencv/cv.h>
#include<opencv/highgui.h>
#include<opencv2/objdetect/objdetect.hpp>
#include<opencv2/highgui/highgui.hpp>
#include<opencv2/imgproc/imgproc.hpp>
#include<vector>

using namespace std;
using namespace cv;

int main(){
    int t = 0;            
    Mat img,img_cinza;
        CascadeClassifier face_cascade, eye_cascade;
    if(!face_cascade.load("/home/pi/Desktop/haarcascades/haarcascade_frontalface_alt.xml")) {
        printf("Error loading cascade file for face");
        return 1;
    }
    if(!eye_cascade.load("/home/pi/Desktop/haarcascades/haarcascade_eye.xml")) {
        printf("Error loading cascade file for eye");
        return 1;
    }
    vector<Rect> eyes, faces;

    VideoCapture cap(0);
    Point centere,centerf;

    while(true){
        cap.read(img);
        

        flip(img,img,1);
        resize(img,img,Size(640,480));
        cvtColor( img, img_cinza, CV_BGR2GRAY );
        equalizeHist( img_cinza, img_cinza);
        face_cascade.detectMultiScale( img_cinza, faces, 1.1, 2, 0|CV_HAAR_SCALE_IMAGE, Size(30, 30) );
        
        if(eyes.size()==0)
            {t++;
            printf("Olhos fechados por %d\n", t);
        }
        else{
            t = 0;    
        }
        for(size_t k=0; k<faces.size(); k++){
            int comp_x=faces[k].x;
            int comp_y=faces[k].y;
            centerf=Point(faces[k].x+int(faces[k].width*0.5), int(faces[k].y+faces[k].height*0.5));
            Mat imgf(img_cinza, faces[k]);
           ellipse(img, centerf, Size(faces[k].width*0.40,faces[k].height*0.55),0,0,360,Scalar(0,0,255),2,8,0);
            eye_cascade.detectMultiScale( imgf, eyes,  1.1, 3, CV_HAAR_FIND_BIGGEST_OBJECT, Size(15, 15), Size(300,300));
                for(size_t j=0; j<eyes.size(); j++){
                    centere=Point(eyes[j].x+eyes[j].width*0.5+comp_x, eyes[j].y+eyes[j].height*0.5+comp_y);
                    if(centere.y<centerf.y)
                        ellipse(img, centere, Size(eyes[j].width*0.5,eyes[j].height*0.5),0,0,360,Scalar(232,168,0),2,8,0);
                }
        }
        namedWindow("Test",CV_WINDOW_AUTOSIZE);
        imshow("Test",img);
        
        int c=waitKey(1);
        if(c==27) 
            break;
    }
    destroyAllWindows();
    return 0;
}


```
