*&---------------------------------------------------------------------*
*& Report ZTESTE009_PAULA
*&---------------------------------------------------------------------*
*&
*&---------------------------------------------------------------------*
REPORT zteste009_paula.

" popup selection
DATA: it_files TYPE filetable,
      ls_files TYPE file_table,
      v_rc     TYPE i.

" gui upload
DATA: lv_file TYPE string.

" type for gui upload
TYPES: BEGIN OF ty_arquivo,
         linha TYPE c LENGTH 2000,
       END OF ty_arquivo.

" table for gui upload
DATA: it_outdata TYPE TABLE OF ty_arquivo,
      ls_outdata TYPE ty_arquivo.

" selecton parameters
PARAMETERS: p_file(1024) TYPE c.

AT SELECTION-SCREEN ON VALUE-REQUEST FOR p_file.


  " method for call selection popup
  CALL METHOD cl_gui_frontend_services=>file_open_dialog
    CHANGING
      file_table              = it_files
      rc                      = v_rc
    EXCEPTIONS
      file_open_dialog_failed = 1
      cntl_error              = 2
      error_no_gui            = 3
      not_supported_by_gui    = 4
      OTHERS                  = 5.

  IF sy-subrc = 0.
    MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
    WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
  ELSE.
************************** identifica o arquivo selecionado e joga para o parametro de seleção
    READ TABLE it_files INTO ls_files INDEX 1.
    p_file = ls_files-filename.
  ENDIF.