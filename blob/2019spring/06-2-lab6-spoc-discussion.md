16.1 �������
1.���̿��ƿ����������ص��ֶ�����Щ����ʲô����»��������޸ģ�
volatile bool need_resched; // bool value: need to be rescheduled to release CPU?

uint32_t wait_state; // waiting state

list_entry_t run_link; // the entry linked in run queue

int time_slice; // time slice for occupying the CPU

skew_heap_entry_t lab6_run_pool; // FOR LAB6 ONLY: the entry in the run pool

uint32_t lab6_stride; // FOR LAB6 ONLY: the current stride of the process

uint32_t lab6_priority; // FOR LAB6 ONLY: the priority of process, set by lab6_set_priority(uint32_t)

2.ucore�ľ����������ݽṹ���Ķ��壿���Ľ����޸ģ�
kern/schedule/sched.c

static struct run_queue __rq;

3.ucore�ĵȴ��������ݽṹ���Ķ��壿���Ľ����޸ģ�
kern/schedule/sched.c

static list_entry_t timer_list;

4.���Ը���ucore�еĵ��ȹ��̡�
�ж���Ӧ���̵߳��ж��ֳ����桢�жϴ������ȴ�������ǰ�߳���ӡ�ѡȡ��һ�������̡߳���һ�������̳߳��ӡ��߳��л������̵߳��ж��ֳ��ָ������̵߳ļ���ִ��


16.2 �����㷨֧�ſ��
1.�����㷨֧�ſ���еĸ�������ָ��Ĺ�����ɶ���ᱻ˭�ں�������µ��ã�
��ʼ����������ѡȡ�����ӡ���ӡ��л�


16.3 ʱ��Ƭ��ת�����㷨
1.ʱ��Ƭ��ת�����㷨����λ��ڵ����㷨֧�ſ��ʵ�ֵģ�
kern/schedule/default_sched.c

struct sched_class default_sched_class

2.ʱ���ж���ε���RR_proc_tick()�ģ�
ʱ���ж�ʱ����ʱ��Ƭ�ļ����������㣨ʱ��Ƭ���꣩ʱ�����ÿɵ��ȱ�־��need_resched����


16.4 stride�����㷨
stride�����㷨��˼·��
����ֵstride

����ֵpass

�Բ���Ϊ���ȼ��Ķ�̬���ȼ������㷨��ÿ��ִ��һ��ʱ��Ƭ��ʱ��Ƭ����ʱ�����ȼ�������Ϊ��������ֵ��

stride�㷨��������ʲô��
��̬���ȼ������㷨

ȷ���ĵ���˳��

�̵߳�ִ��ʱ���벽��ֵ�ĵ���������

stride�����㷨����α���stride�������ģ�
�����޷��������з��űȽϣ��Ӷ����ⲽ��ֵ�޸�ʱ���������

4.�޷��������з��űȽϻ����ʲôЧ����
�޷�������ab��Ϊ����stride
���迪ʼ��ʱ��a=b��֮��b�����ӡ����bû������Ļ�����ʱa-b<0��֮��Ӧ���ֵ�a���ӣ���ʱ�ǳɹ��ġ�

���b����˻�
���ȿ���schedule/default_sched.c����һ�� #define BIG_STRIDE 0x7FFFFFFF
��Ϊstrideÿ�ε��������� BIG_STRIDE / priority������strideÿ�������������ᳬ��BIG_STRIDE 
��ô��Ϊb����ˣ�����b�����֮ǰ��ab��ȣ����޷��Ŵ���0x7FFFFFFF
                               ��b���֮��a��Ȼ����ԭ������0x7FFFFFFF��bС��0x7FFFFFFF
                               ��a-b�޷��Ŵ���0x7FFFFFFF����Ϊb�Ĳ���ֵС��0x7FFFFFFF����Ҳ�����з���С��0����Ȼ�ǳɹ���



��������Ĺؼ�������#define BIG_STRIDE 0x7FFFFFFF
���ֵ�������з������������ֵ������Ǳ�֤stride��������ԭ��
�ٸ����ӣ���BIG_STRIDE����BIG_STRIDE=0xE0000000
��ô��ʼ��a=b=0xE0000000��b��ǰ��0xE0000000��b��Ϊ0xC0000000����ʱ����a-b>0��stride�㷨�ʹ���

5.ʲô��б��(skew heap)��б����stride�㷨��ʵ������ʲô�ã�

б�ѵĶѶ������ȼ���С�Ľڵ㣻б�ѵĺϲ�ʱ�俪��ΪO(logN)��ɾ����С�ڵ�����Ͳ������������ת���ɺϲ�������������С��