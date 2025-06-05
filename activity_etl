CREATE OR REPLACE PACKAGE ACTIVITY_OWNER.REFRESH_APEX_LOGS_pkg 
IS 
 procedure refresh_page_log (p_user varchar2);  
 procedure refresh_workspace_log (p_user varchar2); 
 procedure refresh_debug_messages (p_user varchar2);
 procedure refresh_automation_log (p_user varchar2);
 procedure refresh_mail_log (p_user varchar2);
 procedure refresh_mail_queue (p_user varchar2);
END;
/


GRANT DEBUG ON ACTIVITY_OWNER.REFRESH_APEX_LOGS_PKG TO ACTIVITY_SUPPORT;

CREATE OR REPLACE PACKAGE BODY ACTIVITY_OWNER.REFRESH_APEX_LOGS_pkg 
as 
  procedure refresh_page_log (p_user varchar2)is 
    begin 
      INSERT INTO tapex_workspace_access_log 
            (workspace, application_id, application_name, user_name, authentication_method, application_schema_owner, 
             access_date, ip_address, authentication_result, custom_status_text, workspace_id, created_by, created_date, modified_by, modified_date) 
  SELECT alog.workspace, 
         alog.application_id, 
         alog.application_name, 
         alog.user_name, 
         alog.authentication_method, 
         alog.application_schema_owner, 
         alog.access_date, 
         alog.ip_address, 
         alog.authentication_result, 
         alog.custom_status_text, 
         alog.workspace_id, 
         p_user, 
         sysdate, 
         p_user, 
         sysdate 
    FROM apex_workspace_access_log alog, 
         tapex_workspace_access_log x 
   WHERE alog.access_date = x.access_date(+) 
     AND x.ROWID IS NULL; 
     --AND alog.application_schema_owner = USER; 
     
        EXCEPTION 
     WHEN NO_DATA_FOUND THEN 
       NULL; 
     WHEN OTHERS THEN 
       RAISE; 
  end refresh_page_log; 
   
  procedure refresh_workspace_log (p_user varchar2)is 
  begin 
  INSERT INTO tapex_workspace_activity_log 
            (workspace, apex_user, application_id, application_name, application_schema_owner, page_id, page_name, 
             view_date, think_time, log_context, elapsed_time, rows_queried, ip_address, AGENT, apex_session_id, 
             error_message, error_on_component_type, error_on_component_name, page_view_mode, regions_from_cache, 
             workspace_id, created_by, created_date, modified_by, modified_date) 
  SELECT alog.workspace, 
         alog.apex_user, 
         alog.application_id, 
         alog.application_name, 
         alog.application_schema_owner, 
         alog.page_id, 
         alog.page_name, 
         alog.view_date, 
         alog.think_time, 
         alog.log_context, 
         alog.elapsed_time, 
         alog.rows_queried, 
         alog.ip_address, 
         alog.AGENT, 
         alog.apex_session_id, 
         alog.error_message, 
         alog.error_on_component_type, 
         alog.error_on_component_name, 
         alog.page_view_mode, 
         alog.regions_from_cache, 
         alog.workspace_id, 
         p_user, 
         sysdate, 
         p_user, 
         sysdate 
    FROM apex_workspace_activity_log alog, 
         tapex_workspace_activity_log x 
   WHERE alog.view_date = x.view_date(+) 
     AND alog.apex_session_id = x.apex_session_id(+) 
     --AND alog.application_schema_owner = USER 
     AND x.ROWID IS NULL; 
      
        EXCEPTION 
     WHEN NO_DATA_FOUND THEN 
       NULL; 
     WHEN OTHERS THEN 
       RAISE; 
  end refresh_workspace_log; 
procedure refresh_debug_messages (p_user varchar2) is
  begin
  INSERT INTO tapex_debug_messages(
              id, page_view_id, message_timestamp, elapsed_time, message, application_id, page_id, session_id, apex_user, message_level, workspace_id, call_stack, created_by, created_date, modified_by, modified_date)
  SELECT alog.id,
         alog.page_view_id,
         alog.message_timestamp,
         alog.elapsed_time,
         alog.message,
         alog.application_id,
         alog.page_id,
         alog.session_id,
         alog.apex_user,
         alog.message_level,
         alog.workspace_id,
         alog.call_stack,
         p_user,
         sysdate,
         p_user,
         sysdate
  FROM apex_debug_messages alog,
       tapex_debug_messages x
  WHERE alog.message_timestamp = x.message_timestamp(+)
     AND alog.session_id = x.session_id(+)
     --AND alog.application_schema_owner = USER
     AND x.ROWID IS NULL;
     
  EXCEPTION WHEN NO_DATA_FOUND THEN
       NULL;
  WHEN OTHERS THEN
       RAISE;
end refresh_debug_messages;
procedure refresh_automation_log (p_user varchar2) is
  begin
  INSERT INTO tapex_automation_log(
              id, workspace_id, workspace_name, workspace_display_name, application_id, automation_id, automation_name, is_job, status_code, status, start_timestamp, end_timestamp, successful_row_count, error_row_count)
  SELECT alog.id,
         alog.workspace_id,
         alog.workspace_name,
         alog.workspace_display_name,
         alog.application_id,
         alog.automation_id,
         alog.automation_name,
         alog.is_job,
         alog.status_code,
         alog.status,
         alog.start_timestamp,
         alog.end_timestamp,
         alog.successful_row_count,
         alog.error_row_count
  FROM apex_automation_log alog,
       tapex_automation_log x
  WHERE alog.id = x.id(+)
     --AND alog.application_schema_owner = USER
     AND x.ROWID IS NULL;
     
  EXCEPTION WHEN NO_DATA_FOUND THEN
       NULL;
  WHEN OTHERS THEN
       RAISE;
end refresh_automation_log;
procedure refresh_mail_log (p_user varchar2) is
  begin
  INSERT INTO tapex_mail_log(
              mail_id, mail_message_id, mail_to, mail_from, mail_replyto, mail_subj, mail_cc, mail_bcc, mail_message_created, mail_message_send_begin, mail_message_send_end, mail_body_size, mail_body_html_size, mail_attachment_count, mail_attachment_size, mail_send_error, last_updated_on, app_id, workspace_id, workspace_name)
  SELECT alog.mail_id,
         alog.mail_message_id, 
         alog.mail_to, 
         alog.mail_from, 
         alog.mail_replyto, 
         alog.mail_subj, 
         alog.mail_cc, 
         alog.mail_bcc, 
         alog.mail_message_created, 
         alog.mail_message_send_begin, 
         alog.mail_message_send_end, 
         alog.mail_body_size, 
         alog.mail_body_html_size, 
         alog.mail_attachment_count, 
         alog.mail_attachment_size, 
         alog.mail_send_error, 
         alog.last_updated_on, 
         alog.app_id, 
         alog.workspace_id, 
         alog.workspace_name
  FROM APEX_240100.WWV_FLOW_USER_MAIL_LOG alog,
       tapex_mail_log x
  WHERE alog.mail_id = x.mail_id(+)
     AND alog.mail_message_id = x.mail_message_id(+)
     AND x.ROWID IS NULL;
     
  EXCEPTION WHEN NO_DATA_FOUND THEN
       NULL;
  WHEN OTHERS THEN
       RAISE;
end refresh_mail_log;
procedure refresh_mail_queue (p_user varchar2) is
  begin
  INSERT INTO tapex_mail_queue(
              id, mail_to, mail_from, mail_replyto, mail_subj, mail_cc, mail_bcc, mail_body, mail_body_html, mail_send_count, mail_send_error, last_updated_on, mail_message_created, mail_message_created_by, app_id, workspace_id, workspace_name)
  SELECT alog.id, 
         alog.mail_to, 
         alog.mail_from, 
         alog.mail_replyto, 
         alog.mail_subj, 
         alog.mail_cc, 
         alog.mail_bcc, 
         alog.mail_body, 
         alog.mail_body_html, 
         alog.mail_send_count, 
         alog.mail_send_error, 
         alog.last_updated_on, 
         alog.mail_message_created, 
         alog.mail_message_created_by, 
         alog.app_id, 
         alog.workspace_id, 
         alog.workspace_name
  FROM APEX_240100.WWV_FLOW_USER_MAIL_QUEUE alog,
       tapex_mail_queue x
  WHERE alog.id = x.id(+)
     --AND alog.application_schema_owner = USER
     AND x.ROWID IS NULL;
     
  EXCEPTION WHEN NO_DATA_FOUND THEN
       NULL;
  WHEN OTHERS THEN
       RAISE;
end refresh_mail_queue;
end REFRESH_APEX_LOGS_pkg;
/
