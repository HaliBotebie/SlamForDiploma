# �������ݼ��ϵ�ַ��
https://google-cartographer-ros.readthedocs.io/en/latest/data.html

## �����bagת��δmat�ķ�ʽ��

### ��ROS ��ѹbag ����Ϊmat
1����������b2-2014-12-12-14-41-29.bag���ļ���СΪ46MB;ps:���غ�ֱ����matlab����rosbag������ȡ������ʾ������ROS �½�ѹ
```
rosbag decompress b2-2014-12-12-14-41-29.bag
```
��ѹ���ļ���СΪ460M��


### ��matlab�¶�bag�ļ��е����ݽ�����ȡ�ͱ��档

matlab�ṩ��ros��صĺ�������ȡ���ݣ�����һ�ζ����е����ݽ��д����ᷢ���ڴ治������⡣�����������µĴ����ȡbag�ļ���
```
bag = rosbag('b2-2014-12-12-14-41-29.bag');%��ȡ��������
%��ȡˮƽ�״�topic ����
laser = select(bag, 'Time', ...
            [bag.StartTime bag.EndTime], 'Topic', '/horizontal_laser_2d');
x = readMessages(laser);
```
matlab����(�ڴ治��)��


## ��ˣ�����ѭ���ķ�ʽ��һ��ȡ���ݣ��������£�
����10��Сʱ������㷨Ч���е��.</br>

�汾һ��
```
clear;clc;
bag = rosbag('b2-2014-12-12-14-41-29.bag');%��ȡ��������
%��ȡˮƽ�״�topic ����
 laser = select(bag, 'Time', ...
            [bag.StartTime bag.EndTime], 'Topic', '/horizontal_laser_2d');
        
%% ���ļ��в������ݵĴ�С 
N = laser.NumMessages;%�״���������
x = readMessages(laser,1);
[M,~] = size(x{1,1}.Ranges);
times = zeros(N,1);%ʱ�����
ranges = zeros(N,M);%�������

%% ѭ����ȡ���� �������ȡʱ������ڴ治������
for i=1:N
    temp = readMessages(laser,i);
    times(i) = temp{1,1}.Header.Stamp.Sec;%ʱ��
    ranges_temp = temp{1,1}.Ranges;%�״������1079ά���ݣ�
    for j = 1:M %��֪����������ȡ�����Լ���ѭ��
        laser_echo = ranges_temp(j,1).Echoes;
        [xx,yy] = size(laser_echo);
        if xx*yy<1 %��laser_echoΪ��ʱ��������ǰѭ��
            continue
        end
        ranges(i,j) = laser_echo(1);%�״�����ľ�������
    end
    %��ʾ����
    if mod(i,100)==0
        disp(['�������%��', num2str(i/N*100)]);
    end
end
%���ݱ���Ϊmat�ļ�
save new_laser_data.mat times ranges
```

# �ο���
```
�ٶȲ鶫����ĺ�ǿ�����С����ǽ���ȥGoogle
https://blog.csdn.net/weixin_40712763/article/details/78909608
https://zhuanlan.zhihu.com/p/51577331
```