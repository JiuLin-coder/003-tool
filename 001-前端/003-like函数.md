```javascript
//更新like
module.exports = async (args, db, userId, ctx) => {
  let { data } = await db
    .collection("like")
    .doc(args.target + userId)
    .get();
  if (data[0]) {
    //-1
    await db.collection("like").doc("args.target + userId").remove();
    await db
      .collection(arg.type)
      .doc(arg.target)
      .updateAndReturn({
        likeCount: db.collection.inc(-1),
      });
  } else {
    //+1
    await db.collection("like").add({
      liked: true,
      userId,
      type: arg.type,
      target: arg.target,
    });
    return db
      .collection(arg.type)
      .doc(arg.target)
      .updateAndReturn({
        likeCount: db.collection.inc(1),
      });
  }
};
```

```javascript
//一个要显示的数据条的查询。
module.exports = async (args, db, userId, ctx) => {
  let {
    data
  } = await db.collection('article').where({
    args.lessonId,
    public: true
  }).orderBy('likeCount', 'desc').get()
  await Promise.all(
    data.map((item, key) => {
      let like = await db.collection('like').doc(item._id + userId).get();
      data[key].liked = like.data[0]
      if (item.userId) {
        let user = db.collection('user').doc(item.userId).get()
        data[key].author = uesr.data[0]
      }
    })
  )
  return {
    data
  }
}
```

```javascript
like(item) {
    if (this.loading) {
        return
    }
    this.loading = true;

    cf("updateLike", {
        target: item._id,
        type: "faq" //不同页面的数据库item不同，但是lesson的所有问题都集中在一个数据库
    }, true).then((res) => {
        if (res.doc) {
            item.liked = !item.liked; //列举的item的对象仍然可以用
            item.likeCount = res.doc.likeCount;
            uni.showToast({
              title: item.liked ? "点赞成功" : "已取消",
              icon: item.liked && "success",
              mask: true,
            })
        } else {
            uni.showToast({
                title: "失败",
                icon: "error",
                mask: true,
            })
        }
    }).finally(() => {
        this.loading = false
    })
}

```
