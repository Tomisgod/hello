# hello
text
package com.sjxd.action;

import java.io.IOException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import net.sf.json.JSONArray;

import com.sjxd.dao.ApiTagDefineImpl;
import com.sjxd.model.ApiCustomapiClassDefine;
import com.sjxd.model.ApiCustomapiDefine;
import com.sjxd.model.ApiDataClassDefine;
import com.sjxd.model.ApiDataDefine;
import com.sjxd.model.ApiTagDefine;
 

public class ApiCustomApiClassDefineAction extends SJXDBaseAction{

	/**
	 *  
	 */
	private static final long serialVersionUID = 1L;
	private List<ApiCustomapiClassDefine> accdlist;
	private List<ApiDataClassDefine> adcdlist;
	private List<ApiTagDefine> atdList;
	private List<ApiCustomapiDefine> acdList;
	private List<ApiDataDefine> addList;
	private String apiclass;
	private String apiclassname;
	private String seltype;
	private String sta;
	private String idss;
	private ApiTagDefineImpl apitagdefineDao;
	private Map<String, Long> apiinfo;
	
	/**
	 * 显示定制接口种类定义列表（ApiCustomapiClassDefine）
	 */	
	public String list(){
		
		list = customapiclassdefineDao.findAllIdAndName();
		atdList = apitagdefineDao.findAll();
		ApiCustomapiClassDefine accd = customapiclassdefineDao.findByID(apiclass);
		if(accd != null)
			apiclassname = accd.getCustomApiClassName();	
		json = JSONArray.fromObject(list).toString();
		try {
			response =  getResponse();
			response.setContentType("text/html;charset=UTF-8");
			response.getWriter().write(json);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}		
		return "list";
		
	}
	/**
	 * 显示首页内容
	 * 
	 * **/
	public String listhome(){
		accdlist = customapiclassdefineDao.findAllIdAndName();
		adcdlist = apidataclassdefineDao.findAllIdAndName();
		acdList = apicustomapidefineDao.findAllOrderByPublishtime();
		addList = apidatadefineDao.findAllOrderByPublishTime();
		for(int i=0;i<accdlist.size();i++){
			Long sumapi = apicustomapidefineDao.getBycustomApiClassId(accdlist.get(i).getCustomApiClassId());
			accdlist.get(i).setDescription(String.valueOf(sumapi));
		}
		for(int i=0;i<adcdlist.size();i++){
			Long sumdata = apidatadefineDao.getBydataClassId(adcdlist.get(i).getDataClassId());
			adcdlist.get(i).setDescription(String.valueOf(sumdata));
		}
		return "listhome";
	}
	
	public String apiIntro(){
		apiinfo = new HashMap<String, Long>();
		apiinfo.put("apicount", Long.valueOf(apicustomapidefineDao.findAll().size()));
		apiinfo.put("apiparamcount", Long.valueOf(apiparamdefineDao.findAll().size()));
		adcdlist = apidataclassdefineDao.findAllIdAndName();
		for(int i=0;i<adcdlist.size();i++){
			int sumapi = apidatadefineDao.findBydataClassId(adcdlist.get(i).getDataClassId()).size();
			adcdlist.get(i).setDescription(String.valueOf(sumapi));
			apiinfo.put(adcdlist.get(i).getDataClassId(), Long.valueOf(sumapi));
		}
		return "apiInfo";
	}
	
	
	
	
	public List<ApiCustomapiClassDefine> getAccdlist() {
		return accdlist;
	}
	public void setAccdlist(List<ApiCustomapiClassDefine> accdlist) {
		this.accdlist = accdlist;
	}
	public List<ApiDataClassDefine> getAdcdlist() {
		return adcdlist;
	}
	public void setAdcdlist(List<ApiDataClassDefine> adcdlist) {
		this.adcdlist = adcdlist;
	}
	public String getApiclass() {
		return apiclass;
	}
	public void setApiclass(String apiclass) {
		this.apiclass = apiclass;
	}
	public String getApiclassname() {
		return apiclassname;
	}
	public void setApiclassname(String apiclassname) {
		this.apiclassname = apiclassname;
	}
	public String getSeltype() {
		return seltype;
	}
	public void setSeltype(String seltype) {
		this.seltype = seltype;
	}
	public String getSta() {
		return sta;
	}
	public void setSta(String sta) {
		this.sta = sta;
	}
	public String getIdss() {
		return idss;
	}
	public void setIdss(String idss) {
		this.idss = idss;
	}
	public List<ApiTagDefine> getAtdList() {
		return atdList;
	}
	public void setAtdList(List<ApiTagDefine> atdList) {
		this.atdList = atdList;
	}
	public void setApitagdefineDao(ApiTagDefineImpl apitagdefineDao) {
		this.apitagdefineDao = apitagdefineDao;
	}
	public List<ApiCustomapiDefine> getAcdList() {
		return acdList;
	}
	public void setAcdList(List<ApiCustomapiDefine> acdList) {
		this.acdList = acdList;
	}
	public List<ApiDataDefine> getAddList() {
		return addList;
	}
	public void setAddList(List<ApiDataDefine> addList) {
		this.addList = addList;
	}
	public Map<String, Long> getApiinfo() {
		return apiinfo;
	}
	public void setApiinfo(Map<String, Long> apiinfo) {
		this.apiinfo = apiinfo;
	}
 
	 
 
 
}

