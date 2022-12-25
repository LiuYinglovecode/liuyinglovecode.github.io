---
title: userinfo_interface
date: 2019-11-28 08:51:48
tags:
- DAO
- service
- controller
- interface
categories:
- Java
cover: ../../images/articles/api.jpg
---
## 接口
## 一般需要写5个文件：controller + model + service + serviceImpl + DAO
### 1. controller
  一般由@RequestMapping和@ResponseBody构成
``` bash
@RequestMapping(value = "路径", method = RequestMethod.GET, produces = "名称/json;charset=utf-8")
  @ResponseBody
  public ApiResult appUserList(@RequestParam(value = "入参",defaultValue = "") 入参类型 入参) #多个入参并列添加
  {ApiResult apiResult = new ApiResult();
  PageList<模型model> result = 对应的service.appUserList(入参之间逗号相隔);
  return GenericApiResultUtil.getGenericOKResult(result);}
```
### 2. model
   就是指定数据库的表名以及各栏位的名称，其次是一些get,set方法，模板:
``` bash
  @SqlTable(name = "表名")
  public class model名称(即该java文件名称) {
    @SqlColumn(name = "栏位名")
    private type name;
    public long get栏位名() {
        return 栏位名;
    }

    public UserInfoCache set栏位名(type 栏位名) {
        this.栏位名 = 栏位名;
        return this;
    }
  }
```
### 3. service
  这里指service文件夹里的service.java文件(public interface)和service文件夹里嵌套的文件夹impl里的serviceImpl.java文件。
  interface模板：
``` bash
  public interface service名称 {
  List<类型> 接口名();
  void 接口名(model名称 方法);
  PageList<model类名> 对应controller(参数类型 参数);
  int 接口名(参数类型 参数);
}
```
### 4. serviceImpl
  是service的执行，其模板：
``` bash
  @Service
  @Transactional(propagation = Propagation.REQUIRED)
  public class service名称Impl implements service名称 {
    private final UserInfoCacheDao userInfoCacheDao; # DAO层的撰写
    public service名称Impl(UserInfoCacheDao userInfoCacheDao)
    {this.userInfoCacheDao = userInfoCacheDao;}
    @Override
    public PageList<model> 对应controller(参数类型 参数)
    {return model名称Dao.接口名(参数类型 参数);}
```
### 5. DAO
结构模型：
``` bash
@Component
public class yournameDao extends baseDao<modelname>{
public PageList<modelname> impl中方法的返回值(参数类型 参数)
{String selectSql = "yoursql "; # 封号前要有空格
SqlBuider whereBuilder = new SqlBuilder();
whereBuilder.append("where ...space ").args(参数);
SqlBuilder limitBuilder = new SqlBuilder();
limitBuilder.append("limit ?, ?").args((page - 1) * size, size);
String whereSql = whereBuilder.toString();
String countSql = "yoursql" + whereBuilder.toString();
List<yourmodel> list = ...querryForBeanList(selectSql + whereSql + limitBuilder.toString(), classname.class);
int total = ...querryForObject(countSql, Integer.class);
PageList<yourmodel> PageList = new PageList<>();
pageList.setList(list)
pageList.setTotal(total)
return pageList;
}
}
```
