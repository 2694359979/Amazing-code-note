# Amazing-code-note
Code of CPP language based on OpenCV
//图片轨迹识别#include<opencv2/opencv.hpp>
#include<opencv2/highgui/highgui.hpp>
#include<opencv2/imgproc/imgproc.hpp>
using namespace cv;
using namespace std;
int main()
{
    Mat two,output,ROI,canny,finsh;
    Mat input=imread("/home/suhuolin/桌面/4.jpg");
    //Mat input=imread("/home/suhuolin/桌面/3.jpg");//
    //Mat input=imread("/home/suhuolin/桌面/2.jpg");//
    //Mat input=imread("/home/suhuolin/桌面/1.jpg");//.
    //Mat dst;
    //Mat src1=output.clone();
    //dst.create(src1.size(),src1.type());
    Rect rect(100,43,420,420);
    cvtColor(input,output,COLOR_BGR2GRAY);
    //threshold(output, two, 100, 255, THRESH_BINARY);
    Mat output2;
    blur(output,output2,Size(3,3));
    //void GaussianBlur(output,gao,Size(g_nGaussianBlur));
     Canny(output2,canny,50,200,3);
    ROI=canny(rect);
    Mat huofu=ROI.clone();
    vector<Vec4i>lines;
    HoughLinesP(huofu,lines,1,CV_PI/180,40,100,110);
    cvtColor(huofu,finsh,COLOR_GRAY2BGR);//转回三通道，连线显示。
    for(size_t i=0;i<lines.size();i++)
    {
        Vec4i l=lines[i];
        line(finsh,Point(l[0],l[1]),Point(l[2],l[3]),Scalar(255,88,255),1,LINE_AA);
    }
    //imshow("霍夫",huofu);
    //imshow("霍夫",canny);
    //imshow("转色后的",finsh);//第一步
    Vec4i l=lines[0];
    Point pointright(l[0],l[1]);
    Point pointleft=Point(l[0],l[1]);
    Point pointup=Point(l[0],l[1]);
    Point pointdown=Point(l[0],l[1]);

    for(size_t i=0;i<lines.size();i++)
    {
        Vec4i l=lines[i];
        if(l[0]<=pointleft.x)
        {
            pointleft=Point(l[0],l[1]);
        }
        if(l[2]>pointright.x)
        {
            pointright=Point(l[2],l[3]);
        }
        if(l[1]>l[3]&&l[3]<pointup.y)
        {
            pointup=Point(l[2],l[3]);
        }
        if(l[1]<l[3]&&l[1]<pointup.y)
        {
            pointup=Point(l[0],l[1]);
        }
        if(l[1]<l[3]&&l[3]>pointdown.y)
        {
            pointdown=Point(l[2],l[3]);
        }
        if(l[1]>l[3]&&l[1]>pointdown.y)
        {
            pointdown=Point(l[0],l[1]);
        }
    }
    line(finsh,Point(pointdown.x,pointdown.y),Point(pointright.x,pointright.y),Scalar(22,222,22),1,LINE_AA);//第二步
    int a=1,b=1;
    if(pointdown.y!=pointleft.y&&pointdown.x!=pointleft.x&&pointdown.y!=pointright.y&&pointdown.x!=pointright.x&&pointup.y!=pointleft.y&&pointup.x!=pointleft.x&&pointup.y!=pointright.y&&pointup.x!=pointright.x)
    {
    if((pointdown.y-pointleft.y)/(pointdown.x-pointleft.x)>0.5&&(pointdown.y-pointleft.y)/(pointdown.x-pointleft.x)<100&&(pointdown.y-pointright.y)/(pointdown.x-pointright.x)<-0.5&&(pointdown.y-pointright.y)/(pointdown.x-pointright.x)>-100)
    {
        putText(input,"ten cross",Point(20,125),FONT_HERSHEY_PLAIN,2,Scalar(100,0,0),2,8,0);

         imshow("wan",input) ;  //十字
          a=2;
    }
    }
    if(a==1)
    {
        if(pointdown.y!=pointleft.y&&pointdown.x!=pointleft.x)
        {
        if((pointdown.y-pointleft.y)/(pointdown.x-pointleft.x)>0.5&&(pointdown.y-pointleft.y)/(pointdown.x-pointleft.x)<10)
        {
            putText(input,"left",Point(20,125),FONT_HERSHEY_PLAIN,2,Scalar(100,0,0),2,8,0);
            imshow("wan",input) ;//向左
            b=2;
        }
        }
        if(b==1)
        {
        if(pointright.x-pointleft.x>50)
        {
            putText(input,"right",Point(20,125),FONT_HERSHEY_PLAIN,2,Scalar(100,0,0),2,8,0);
            imshow("wan",input) ;//向右 
        }
        else
        {
            putText(input,"stright",Point(20,125),FONT_HERSHEY_PLAIN,2,Scalar(100,0,0),2,8,0);
            imshow("wan",input) ;
        }
        }
    }
    waitKey(0);
    return 0;
}
