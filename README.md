# ChallengeRepo


Aura Challenge: 

<aura:component implements="flexipage:availableForRecordHome,force:hasRecordId" access="global">
	 <aura:attribute name="fields" type="String[]" default="['Name','AnnualRevenue','Industry', 'Website']" />
            <aura:attribute name="recordId" type="String"/>
            <lightning:notificationsLibrary aura:id="notifLib"/>

    <lightning:card>
            <lightning:recordForm
                    objectApiName="Account"
                    fields="{!v.fields}"
                    onsuccess="{!c.handleSuccess}" />  	
     </lightning:card>
    
    
    <aura:attribute name="data" type="Object"/>
    <aura:attribute name="columns" type="List"/>

    <aura:handler name="init" value="{! this }" action="{! c.init }"/>
    
    <lightning:card>
    	<lightning:datatable
                keyField="id"
                data="{!v.data }"
                columns="{! v.columns }"
               />
        </lightning:card>  
</aura:component>









Batch Apex Challenge:

public class BatchableApexJob implements Schedulable, Database.Batchable<sObject> {
    
    
    public Database.QueryLocator start(Database.BatchableContext bc) {
       
        String query = 'SELECT Id, Name, CloseDate FROM Opportunity WHERE IsClosed = False AND CloseDate <= dateadd(month, -6, getdate())'      
        return Database.getQueryLocator(query);
    }
    public void execute(Database.BatchableContext bc, List<Opportunity> scope){        
              delete scope;
    }
    
    public void finish(Database.BatchableContext bc){
        System.debug(recordsProcessed + ' records processed. WooHoo!');
    }
            
    BatchableApexJob wow = new BatchableApexJob();    
    String soc = '00 00 14 13 23';
    System.schedule('Remove Opportunity Records', soc, wow);  
          
}
