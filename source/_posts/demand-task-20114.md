---
title: Demand delete interface
date: 2020-2-18 22:30:29
tags:
- spring
- controller
- interface
categories:
- Java
---
## 两种删除方式
### 1. 第一种只逻辑删除demand表
#### * DemandController
``` bash
    import com.yunlu.core.api.annotation.ApiDeclare;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    private static final Logger LOGGER = LoggerFactory.getLogger(DemandController.class);

     @RequestMapping(value = "/delete", method = RequestMethod.POST, produces = "application/json;charset=utf-8")
     @ResponseBody
     @ApiDeclare(alias = "ncov.demand.delete")
     public ApiResult<Boolean> delete(@RequestBody List<Long> idList)
     {
         if (!(idList.size() > 0)) {
             return ResultUtil.getErrorResult(ResultUtil.ERROR_PARAMS_CODE, "未传参数");
         }
         try {
             boolean flag = demandService.delete(idList);
             return ResultUtil.getOKResult(flag);
         } catch (Exception e) {
             LOGGER.error(e.getMessage());
             return ResultUtil.getErrorResult(ResultUtil.ERROR_UNKNOWN_FAIL_CODE, "fail");
         }
     }
```
#### * DemandService
``` bash
    import java.util.List;

    boolean delete(List<Long> ids);
```
#### * DemandServiceImpl
``` bash
    import com.yunlu.core.data.sql.JdbcUtil;
    import com.yunlu.core.data.sql.SqlBuilder;
    import org.apache.commons.beanutils.ConvertUtils;

    import java.util.List;

    private static final String CONFIG = "ncov_jdbc";

     @Override
     public boolean delete(List<Long> ids) {
         if (ids.size() > 0) {
             String idStr = "";
             for (int i = 0; i < ids.size(); i++) {
                 idStr += ids.get(i) + ",";
             }
             idStr = idStr.substring(0, idStr.length() - 1);
             String[] id = idStr.split(",");
             Long[] idNum = (Long[]) ConvertUtils.convert(id, Long.class);
             // 逻辑删除
             SqlBuilder sqlBuilder1 = new SqlBuilder("UPDATE demand SET deleted=1 where demand_id in (?)").arg(idNum);
             long count = JdbcUtil.getInstance(CONFIG).update(sqlBuilder1.toString());
             if (count > 0 && count == id.length) {
                 return true;
             }
         }
         return false;
     }
```

### 2. 第二种逻辑删除demand表，物理删除demand_product表
#### * DemandController
``` bash
    @RequestMapping(value = "/demand", produces = "application/json;charset=utf-8", method = RequestMethod.DELETE)
    public ApiResult<String> deleteDemand(@RequestParam("demand_id") int demandId) {
        try {
            demandService.deleteDemand(demandId);
            return GenericApiResultUtil.getGenericOKResult("Delete demand: " + demandId + " successfully!");
        } catch (Exception e) {
            throw new ResultException(ResultUtil.ERROR_DELETE_FAIL_CODE, e.getMessage());
        }
    }
```
#### * DemandDao
``` bash
    /**
     * 删除分类时，demand_product表和demand表中的记录要同步删除
     *
     * @param demandId 要删除的分类的id
     */
    @Transactional(transactionManager = "transactionManager", propagation = Propagation.REQUIRED, isolation = Isolation.READ_COMMITTED, rollbackFor = Exception.class)
    public void deleteDemand(int demandId) throws Exception {
        deleteDemandProduct(demandId);
        deleteDemand0(demandId);
    }

    private void deleteDemand0(int demandId) {
        jdbcTemplate.update(new PreparedStatementCreatorImpl("UPDATE demand SET deleted=1 where demand_id in (?)",
                new Object[] { demandId }));
    }

    private void deleteDemandProduct(int demandId) {
        jdbcTemplate.update(new PreparedStatementCreatorImpl("delete from `demand_product` where demand_id = ?",
                new Object[] { demandId }));
    }
```

#### * DemandService
``` bash
    /**
     * delete demand
     * 
     * @param demandId the id of demand to delete
     * @throws Exception throw Exception when failed to delete
     */
    void deleteDemand(int demandId) throws Exception;
```

#### * DemandServiceImpl
``` bash
    @Override
    public void deleteDemand(int demandId) throws Exception {
        try {
            demandDao.deleteDemand(demandId);
        } catch (Exception e) {
            LOGGER.error(e.getMessage());
            throw e;
        }
    }
```
