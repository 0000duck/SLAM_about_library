---

# 0 PCL��̹淶

### 0 PCL�����淶

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

# 1 PCL�������Լ�����Լ����������


### 1.1 PointXYZ-float x, y, z;
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

### 1.2 PointXYZI-float x, y, z, intensity;
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

### 1.3 PointXYZRGBA-float x, y, z; uint32_t rgba;
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

### 1.4 �򵥵Ķ�ά x-y point �ṹ
```c
struct
{
    float x;
    float y;
};
```

### 1.5 InterestPoint-float x, y, z, strength;
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

### 1.6 Normal-float normal[3], curvature;
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

### 1.7 PointNormal-float x, y, z; float normal[3], curvature;
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

### 1.8 PointXYZRGBNormal-float x, y, z, rgb, normal[3], curvature;
PointXYZRGBNormal�洢��XYZ���ݺ�RGB����,���һ��������ߺ�����

### 1.9 PointXYZINormal-float x, y, z, intensity, normal[3], curvature;
PointXYZINormal�洢��XYZ���ݺ�ǿ��ֵ,���һ��������淨�ߺ�����

### 1.10 PointWithRange-float x, y, z(union with float point[4]), range;
����range����������õ��ӵ㵽�ɼ���ľ��������,������PointXYZI����

### 1.11 PointWithViewpoint-float x, y, z, vp_z, vp_y, vp_z;
vp_x vp_y vp_z ��ʾ�ӵ�

### 1.12 omentInvariants-float j1, j2, j3;
����������������Ƭ����������ص�point����
```c
struct
{
    float j1, j2, j3;
};

```

### 1.13 PrincipalRadiiRSD-float r_min, r_max;
���������������RSD�뾶��point����

### 1.14 Boundary-uint8_t boundary_point;
�洢һ�����Ƿ�λ������߽��ϵļ�point����

### 1.15 PrincipalCurvatures-float principal_curvature[3], pcl, pc2;

### 1.16 PFHSignature125-float pfh[125];
�����������PFH(������ֱ��ͼ)�ļ�point����

### 1.17 FPFHSignature33-float fpfh[33];
�����������FPFH(���ٵ�����ֱ��ͼ)�ļ�point����
```c
struct
{
    float histogram[33];
};
```

### 1.18 VFHSignature308-float vfh[308];
����������VFH(�ӵ�����ֱ��ͼ)�ļ�point����
```c
struct
{
    float histogram[308];
};
```

# 2 ��������
PCL�ڲ�֧�ֶԳ���3D��ʽ�ļ��Ĵ���洢����,�Լ���PCD�ڲ���ʽ֮��Ļ���ת��.

֧��3D���ƴ洢���ļ�����:

PCD(��������)

PLY ������ļ���ʽ

STL ��Ҫ����CAD����

OBJ ����ѧ�϶�����ļ���ʽ

X3D ����ISO��׼�Ļ���XML���ļ���ʽ

### 2.1 PCD�ļ�IO����

##### 2.1.1 ��PCD�ļ��ж�ȡ��������

```c
#include <pcl/io/pcd_io.h> //������PCD�����������������
#include <pcl/point_types.h> //������PointT���ͽṹ�������

pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(new pcl::PointCloud<pcl::PointXYZ>);
```
����һ��PointCloud<PointXYZ>boost����ָ�벢����ʵ����.

```c
if(pcl::io::loadPCDFile<pcl::PointXYZ>("test_pcd.pcd", *cloud) == -1)
//���ļ�
{
    PCL_ERROR("Couldn't read file test_pcd.pcd\n");
}
```
##### 2.1.2 ��PCD�ļ�д���������

```c
// ��������
pcl::PointCloud<pcl::PointXYZ> cloud;

cloud.width = 5;
cloud.height = 1;
cloud.is_dense = false;
cloud.points.resize(cloud.width * cloud.height);
for(size_t i = 0; i < cloud.points.size();++i)
{
    cloud.points[i].x = 1024 * rand() / (RAND_MAX + 1.0f);
    cloud.points[i].y = 1024 * rand() / (RAND_MAX + 1.0f);
    cloud.points[i].z = 1024 * rand() / (RAND_MAX + 1.0f);
}

pcl::io::savePCDFileASSCII("test_pcd.pcd",cloud);
```

##### 2.1.3 ����ƴ��
�����ֶ����ͺ�ά����ͬ�ĵ��ƽ���ƴ�� cloud_c = cloud_a + cloud_b

������ͬ�ֶεĵ��ƽ���ƴ�� pcl::concatenateFields(cloud_a,n_cloud_b,p_n_cloud_c);

##### 2.1.4 PLY LAS�����ݸ�ʽ��PCD��ת��
LAS�����ڼ����״����ݽ����ı�׼�ļ���ʽ.

'''c
pcl::PCLPointCloud2 cloud;
pcl::PLYReader reader;
reader.read(filename, cloud);
pcl::PCDWriter writer;
writer.write(filename, cloud, Eigen::Vector4f::Zero(), Eigen::Quaternionf::Identity(), format);

'''


# 3Kd����˲���

### 3.1 K��������
�������ĳ���������n����
```c
kdtree.nearestKSearch(searchPoint, K, pointIdxNKNSearch, pointNKNSquaredDistance) 
```

��������ĳ����С��һ����Χ�ĵ� 

```c
pcl::KdTreeFLANN<pcl::PointXYZ> kdtree; //����һ��kdtree����
kdtree.nearestKSearch(searchPoint, K, pointIdxNKNSearch, pointNKNSquaredDistance)
```

### 3.2 �˲����ռ�ָ���е���ѹ��

Octree�˲�����һ������������ά�ռ����״���ݽṹ��
�˲�����ÿ���ڵ��ʾһ������������Ԫ�أ�ÿ���ڵ��а˸��ӽڵ㣬
���˸��ӽڵ�����ʾ�����Ԫ�ؼ���һ��͵��ڸ��ڵ�������
Octreeģ�ͣ��ֳ�Ϊ�˲���ģ�ͣ�����Ϊ�����Ļ���
������һ�ڵ���ӽڵ�ǡ��ֻ���а˸����������Ҳ�����ӽڵ㲻����0��8�������Ŀ��
Log8(�����ڵ�������Ʒ��)��ʱ���ھͿ��ҵ���ҡ�
��ˣ��˲�����������3D�ռ��еĳ����������Ժܿ��֪��������3D�����е�λ�ã�
����������������Ƿ�����ײ�Լ��Ƿ��ڿ��ӷ�Χ�ڡ�

