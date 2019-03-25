---

# 1 PCL��̹淶

### 1 PCL�����淶

##### �ļ�����
ͷ�ļ� .h
ģ���� .hpp
Դ�ļ� .cpp

##### Ŀ¼����

ͷ�ļ� include/
ģ���� include/impl
Դ�ļ� src/

##### include���
ͬһĿ¼�� include��˫����
��������ü�����

����:
```c++
#include <pcl/module_name/file_name.h>
#include <pcl/module/impl/file_name.hpp>
#include "file_name.cpp"
```

# 2 PCL�������Լ�����Լ����������

PointXYZ-float x, y, z;
```c
union
{
    float data[4];
    struct  
    {
        float x;
        float y;
        float z; 
    };
};
```

PointXYZI-float x, y, z, intensity;
```c
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
union
{
    struct
    {
        float intensity;
    } 
    float data_c[4];
};

```

PointXYZRGBA-float x, y, z; uint32_t rgba;
```c
union
{
    float data[4];
    struct 
    {
        float x;
        float y;
        float z;
    };
};
union
{
    struct
    {
        uint32_t rgba; 
    };
    float data_c[4];
}
```

�򵥵Ķ�ά x-y point �ṹ
```c
struct
{
    float x;
    float y;
};
```

InterestPoint-float x, y, z, strength;
```c
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
union
{
    struct
    {
        float strength;
    };
    float data_c[4];
}
```

Normal-float normal[3], curvature;
Normal�ṹ���ʾ�������������������ϵķ��߷���,�Լ���Ӧ���ʵĲ���ֵ
�û����ʷ������ĵ�һ������,����ͨ��points[i].data_n[0]  points[i].normal[0] ����point[i].normal_x
```c
union
{
    float data_n[4];
    float normal[3]; 
    struct
    {
        float normal_x;
        float normal_y;
        float normal_z;
    };
};
union
{
    struct
    {
        float curvature;
    };
    float data_c[4];
};
```

PointNormal-float x, y, z; float normal[3], curvature;
�洢XYZ����,�������Ӧ�ķ��ߺ�����
```c
union
{
    float data[4];
    struct
    {
        float x;
        float y;
        float z;
    };
};
union
{
    float data_n[4];
    float normal[3];
    struct
    {
        float normal_x;
        float normal_y;
        float normal_z;
    };
};
union
{
    struct
    {
        float curvature;
    };
    float data_c[4];
};
```

PointXYZRGBNormal-float x, y, z, rgb, normal[3], curvature;
PointXYZRGBNormal�洢��XYZ���ݺ�RGB����,���һ��������ߺ�����

PointXYZINormal-float x, y, z, intensity, normal[3], curvature;
PointXYZINormal�洢��XYZ���ݺ�ǿ��ֵ,���һ��������淨�ߺ�����

PointWithRange-float x, y, z(union with float point[4]), range;
����range����������õ��ӵ㵽�ɼ���ľ��������,������PointXYZI����

PointWithViewpoint-float x, y, z, vp_z, vp_y, vp_z;
vp_x vp_y vp_z ��ʾ�ӵ�

MomentInvariants-float j1, j2, j3;
����������������Ƭ����������ص�point����
```c
struct
{
    float j1, j2, j3;
};

```

PrincipalRadiiRSD-float r_min, r_max;
���������������RSD�뾶��point����

Boundary-uint8_t boundary_point;
�洢һ�����Ƿ�λ������߽��ϵļ�point����

PrincipalCurvatures-float principal_curvature[3], pcl, pc2;

PFHSignature125-float pfh[125];
�����������PFH(������ֱ��ͼ)�ļ�point����

FPFHSignature33-float fpfh[33];
�����������FPFH(���ٵ�����ֱ��ͼ)�ļ�point����
```c
struct
{
    float histogram[33];
};
```

VFHSignature308-float vfh[308];
����������VFH(�ӵ�����ֱ��ͼ)�ļ�point����
```c
struct
{
    float histogram[308];
};
```

# 3 ��������
PCL�ڲ�֧�ֶԳ���3D��ʽ�ļ��Ĵ���洢����,�Լ���PCD�ڲ���ʽ֮��Ļ���ת��.

