# �������ʽ����Ԫ��ת��

geometryTransition�����������ʽ����Ԫ��ת�����������ʾ�л��������ṩƽ������Ч����ͨ��transition�����ṩ��opacity��scale��ת����Ч��geometryTransitionͨ��id��in/out���(inָ�볡�����outָ�������)��ʹ�����ԭ��������transition�����ڿռ�λ���Ϸ�����ϵ���Ӷ����Ӿ������ɳ������λ���������볡���λ�á�

> **˵����**
>
> ��API Version 10��ʼ֧�֡������汾�����������ݣ�������ϽǱ굥����Ǹ����ݵ���ʼ�汾��

## ����

| ����               | �������� | ��������                                                     |
| ------------------ | -------- | ------------------------------------------------------------ |
| geometryTransition | string   | ����geometryTransition��id���������ð󶨹�ϵ��id��Ϊ���ַ���""������󶨹�ϵ������빲����Ϊ��id��̬�޸Ŀ����½����󶨹�ϵ��ͬһ��idֻ��������������ҷֱ���in/out����� |

**˵����**

geometryTransition�������animateToʹ�ò��ж���Ч������Чʱ�������߸���animateTo�е����ã���֧��.animation��ʽ������

## ʾ��

```ts
// xxx.ets
@Entry
@Component
struct Index {
  @State isShow: boolean = false

  build() {
    Stack({ alignContent: Alignment.Center }) {
      if (this.isShow) {
        Image($r('app.media.pic'))
          .autoResize(false)
          .clip(true)
          .width(300)
          .height(400)
          .offset({ y: 100 })
          .geometryTransition("picture")
          .transition(TransitionEffect.OPACITY)
      } else {
        // geometryTransition�˴��󶨵�����������ô�����ڵ����������Ϊ��Բ��ָ��游�����仯��
        // �׶������Ϊ��˵����Բ���Լ������
        Column() {
          Column() {
            Image($r('app.media.icon'))
              .width('100%').height('100%')
          }.width('100%').height('100%')
        }
        .width(80)
        .height(80)
        // geometryTransition��ͬ��Բ�ǣ���������geometryTransition�󶨴����˴��󶨵�������
        // �������������Բ��ͬ����������������ڲ��������borderRadius
        .borderRadius(20)
        .clip(true)
        .geometryTransition("picture")
        // transition��֤����볡������������������������ת��Ч��
        .transition(TransitionEffect.OPACITY)
      }
    }
    .onClick(() => {
      animateTo({ duration: 1000 }, () => {
        this.isShow = !this.isShow
      })
    })
  }
}
```
![geometrytransition](figures/geometrytransition.gif)


