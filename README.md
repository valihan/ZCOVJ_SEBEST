# ZCOVJ_SEBEST
Динамическая таблица


Подробное описание работы с динамическими таблицами доступно по адресу
https://jumadilov.kz/2021/07/23/%d0%b4%d0%b8%d0%bd%d0%b0%d0%bc%d0%b8%d1%87%d0%b5%d1%81%d0%ba%d0%b8%d0%b5-%d1%82%d0%b0%d0%b1%d0%bb%d0%b8%d1%86%d1%8b/

В ZCOVJ_SEBEST_ALV:

  CALL METHOD cl_alv_table_create=>create_dynamic_table
    EXPORTING
      i_style_table             = 'X'
      it_fieldcatalog           = gt_dyn_fcat
    IMPORTING
      ep_table                  = gt_dyn_table
    EXCEPTIONS
      generate_subpool_dir_full = 1
      OTHERS                    = 2.

  IF sy-subrc EQ 0.
* Assign the new table to field symbol
    ASSIGN gt_dyn_table->* TO <gfs_dyn_table>.
* Create dynamic work area for the dynamic table
    CREATE DATA gs_line LIKE LINE OF <gfs_dyn_table>.
    CREATE DATA gs_line1 LIKE LINE OF <gfs_dyn_table>.
    ASSIGN gs_line->* TO <gfs_line>.
    ASSIGN gs_line1->* TO <gfs_line_i>.
  ENDIF.


И потом выводится в ALV
  CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY'
    EXPORTING
      i_callback_program       = sy-repid
      i_callback_pf_status_set = 'SET_PF_STATUS'
      i_callback_user_command  = 'USER_COMMAND'
      i_callback_top_of_page   = 'TOP_10'
      it_fieldcat              = lt_alv_fieldcat
      i_default                = 'X'
      i_save                   = 'A'
      is_layout                = ls_layout
    TABLES
      t_outtab                 = <gfs_dyn_table>.
