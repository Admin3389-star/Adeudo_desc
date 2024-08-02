<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<!-- saved from url=(0014)about:internet -->
<html lang="en"><head id="head"><meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><title>ICV - Estado de Cuenta</title>
    
        <!-- Required meta tags -->
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        
        <!-- IE 7 Sin compatibilidad -->    
        <meta http-equiv="X-UA-Compatible" content="IE=edge">                       
        
        <!-- Bootstrap CSS -->
        <link rel="stylesheet" href="./bootstrap.min.css" integrity="sha384-PsH8R72JQ3SOdhVi3uxftmaW6Vc51MKb0q5P2rRUpPvrszuE4W1povHYgTpBfshb" crossorigin="anonymous"><link rel="stylesheet" href="./font-awesome.min.css" type="text/css">

        <!-- Bootstrap core CSS -->
        <link href="./normalize.css" rel="stylesheet" type="text/css"><link href="./mensajes.css" rel="stylesheet" type="text/css"><link href="./layout.css" rel="stylesheet" type="text/css"><link href="./menu.css" rel="stylesheet" type="text/css"><link href="./controls.css" rel="stylesheet" type="text/css"><link href="./gridView.css" rel="stylesheet" type="text/css"><link href="./calendarExtender.css" rel="stylesheet" type="text/css">

        <!-- Styles -->
        
    <link href="./wucJModal.css" rel="stylesheet" type="text/css">
    <style type="text/css">
        .table-hover tbody tr:hover td, .table-hover tbody tr:hover th {
            background-color: #color;
        }

        html {
            font-size: .8rem;
        }

        .table td {
            border-color: transparent;
        }

        .table thead th {
            border-color: transparent;
        }

        @include media-breakpoint-up(sm) {
            html {
                font-size: 1.2rem;
            }
        }

        @include media-breakpoint-up(md) {
            html {
                font-size: 1.4rem;
            }
        }

        @include media-breakpoint-up(lg) {
            html {
                font-size: 1.6rem;
            }
        }
    </style>
                           
        
        <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
        <!--[if lt IE 9]>
          <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
          <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
        <![endif]-->  
        
        <script src="./jquery-3.2.1.min.js.descarga"></script>
        <script src="./popper.min(1).js.descarga" integrity="sha384-vFJXuSJphROIrBnz7yo7oB41mKfc8JzQZiCq4NCceLEaO4IHwicKwpJf9c9IpFgh" crossorigin="anonymous"></script>
        <script src="./bootstrap.min(1).js.descarga" integrity="sha384-alpBpkh1PFOepccYVYDB4do5UnbKysX5WZXm3XxPqe5iKTfUKjNkCk9SaVuEZflJ" crossorigin="anonymous"></script>

        <!-- Scripts -->
        
    <script src="./mooDal.js.descarga" type="text/javascript"></script>
    <script src="./Common.js.descarga" type="text/javascript"></script>
    <script type="text/javascript" src="./JsBarcode.code128.min.js.descarga"></script>
    <!--<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/jsbarcode@3.8.0/dist/JsBarcode.all.min.js"></script>-->
    <script type="text/javascript">
        $(document).ready(function () {
            var ST = getParameterByName("ST");

            if (ST != "") {
                $('#CPH_PRINCIPAL_chkDonativo').prop('checked', false);
                $('#btnRegresar').prop('href', 'ConsultaEstadoCuenta.aspx?ST=1');
            }
        });

        function showCodigoBarras() {
            {
                $("#codigoBarrasLR").JsBarcode($("#CPH_PRINCIPAL_lblLR").html().replace(/\s/g, ""), { width: 1, height: 25, quite: 10, format: "CODE128", displayValue: false, background: '#00000000' });
                var PlacaO = document.getElementById('CPH_PRINCIPAL_lblPlaca').innerText;
                document.getElementById('CPH_PRINCIPAL_lblPlacaO').innerText = PlacaO;
				document.getElementById('CPH_PRINCIPAL_lblPlacaO2').innerText = PlacaO;

            };
            var ST = getParameterByName("ST");

            if (ST != "") {
                $("#divStatus").show();
                $("#divBancos").hide();

                var _href = $("#btnRegresar").attr("href");
                $("#btnRegresar").attr("href", _href + '?ST=1');
            }

            // Verificar que estatus tiene para desplegar mensaje de Tarjeta de Circulacion.
            if ($("#CPH_PRINCIPAL_lblEstatus").html() == 0) {
                $("#divTCIndefinidaMsg").html("<strong>Usted cuenta con una tarjeta de circulacion condicionada al pago de Refrendo Anual.</strong>");
            }
            else {
                $("#divTCIndefinidaMsg").html("<strong><u>Usted NO cuenta con una tarjeta de circulación condicionada al pago de Refrendo Anual. Podrás efectuar tu pago y recibir tu documentación en la <a href='http://www.nl.gob.mx/listado-ubicaciones/59332' target='_blank'>delegación del ICV mas cercana</a>, 4 días hábiles posteriores a tu pago. </u></strong>");
            }
        }
    </script>
    <script type="text/javascript">
        function popitup() {
            var url = document.getElementById("CPH_PRINCIPAL_hfRef");
            newwindow = window.open(url.value, 'name', 'height=600,width=600');
            if (window.focus) { newwindow.focus() }
            return false;
        }
        function ScrollToBottom() {
            $('html,body').animate({
                scrollTop: $("#btnEnviarCorreo").offset().top
            },
                'slow');
        }
        function EnviarPorCorreo() {
            var valida = false;
            $("#msgErrorMail").hide();
            $("#msgSuccessMail").hide();

            if (trim($('#txtCorreo')[0].value) == "") {
                $("#msgErrorMail").html("Capture la direccion de correo a donde se envía");
                $("#msgErrorMail").show();
            }
            else if (!CorreoValido(trim($('#txtCorreo')[0].value))) {
                $("#msgErrorMail").html("La direccion de correo no es valida");
                $("#msgErrorMail").show();
            }
            else {
                valida = true;
                $("#msgErrorMail").hide();
            }

            if (valida) {
                $.ajax({
                    type: "POST",
                    url: "ConsultaEstadoCuenta.aspx/EnviarCorreo",
                    data: "{name: '" + $('#txtCorreo')[0].value + "', body: '" + $('#contenidoEstadoCuenta').html() + "' }",
                    contentType: "application/json; charset=utf-8",
                    dataType: "json",
                    beforeSend: function () {
                        $("#btnEnviarCorreo").addClass("disabled");
                        $("#txtCorreo").addClass("disabled");
                        $("#btnEnviarCorreo").html("<i class=\"fa fa-refresh fa-spin fa-fw\"></i> Enviando...");
                    },
                    success: OnSuccess,
                    error: function (xhr, errorType, exception) {
                        var responseText = jQuery.parseJSON(xhr.responseText);

                        $("#msgErrorMail").html(responseText.Message);
                        $("#msgErrorMail").show();
                    },
                    complete: function () {
                        $("#btnEnviarCorreo").removeClass("disabled");
                        $("#txtCorreo").removeClass("disabled");
                        $("#btnEnviarCorreo").html("<i class=\"fa fa-envelope\"></i> Enviar al correo");
                        ScrollToBottom();
                    }
                });
            }
            ScrollToBottom();
        }

        function OnSuccess(response) {
            $("#msgSuccessMail").show();
            $('#txtCorreo')[0].value = "";
        }
    </script>


        <!--[if IE]>
        <script src="~/scripts/DD_roundies.js"></script>
        <script type="text/javascript" src="~/scripts/IEFix.js"></script>
        <![endif]-->

        <style>
                 /* Large desktop */
                @media (min-width: 1200px)  
                {
                	.actualBootSize
                	{
                		background-color:#e0e0e0;
                		}
                		
                    .actualBootSize span::after { 
                        content: " - Extra Large";
                    }
                }

                /* Portrait tablet to landscape and desktop */
                @media (min-width: 992px) and (max-width: 1199px)  
                {
                	.actualBootSize
                	{
                		background-color:#e0e0e0;
                		}
                		
                    .actualBootSize span::after { 
                        content: " - Large";
                    }
                }
                	
                /* Portrait tablet to landscape and desktop */
                @media (min-width: 768px) and (max-width: 991px)  
                {
                	.actualBootSize
                	{
                		background-color:#e0e0e0;
                		}
                		
                    .actualBootSize span::after { 
                        content: " - Medium";
                    }
                }

                /* Landscape phone to portrait tablet */
                @media (min-width: 576px) and (max-width: 767px)  
                {
                	.actualBootSize
                	{
                		background-color:#e0e0e0;
                		}
                		
                    .actualBootSize span::after { 
                        content: " - Small";
                    }
                	}

                /* Landscape phones and down */
                @media (max-width: 575px)  
                {
                	.actualBootSize
                	{
                		background-color:#e0e0e0;
                		}
                		
                    .actualBootSize span::after { 
                        content: " - Extra Small";
                    }
                }
        </style>
</head>
<body style="font-family: HelveticaNeue-Light,Helvetica Neue Light,Helvetica Neue,Helvetica,Arial,Lucida Grande,sans-serif">
    <div id="main">
        <!--<div class="actualBootSize">Actual BootSize<span></span></div>-->
        <form method="post" action="https://www.icvnl.gob.mx:1034/edocuenta24/Consultaestadocuenta.aspx" id="form_main">
<div class="aspNetHidden">
<input type="hidden" name="__EVENTTARGET" id="__EVENTTARGET" value="ctl00$CPH_PRINCIPAL$btnConsultar">
<input type="hidden" name="__EVENTARGUMENT" id="__EVENTARGUMENT" value="">

</div>

<script type="text/javascript">
//<![CDATA[
var theForm = document.forms['form_main'];
if (!theForm) {
    theForm = document.form_main;
}
function __doPostBack(eventTarget, eventArgument) {
    if (!theForm.onsubmit || (theForm.onsubmit() != false)) {
        theForm.__EVENTTARGET.value = eventTarget;
        theForm.__EVENTARGUMENT.value = eventArgument;
        theForm.submit();
    }
}
//]]>
</script>


<script src="./WebResource.axd" type="text/javascript"></script>


<script src="./ScriptResource.axd" type="text/javascript"></script>
<script type="text/javascript">
//<![CDATA[
if (typeof(Sys) === 'undefined') throw new Error('Error al cargar el marco de trabajo de cliente ASP.NET Ajax.');
//]]>
</script>

<script src="./ScriptResource(1).axd" type="text/javascript"></script>
<div class="aspNetHidden">

	
	
</div>
            <script type="text/javascript">
//<![CDATA[
Sys.WebForms.PageRequestManager._initialize('ctl00$sm_main', 'form_main', ['tctl00$CPH_PRINCIPAL$UpdatePanel1','CPH_PRINCIPAL_UpdatePanel1','tctl00$CPH_PRINCIPAL$upMain','CPH_PRINCIPAL_upMain'], [], [], 90, 'ctl00');
//]]>
</script>

            <div>
                                          
            </div>
                
    <button type="button" onclick="popitup()" class="btn btn-danger  displayNone">
        Pagar</button>
    <div id="CPH_PRINCIPAL_UpdatePanel1">
            <input type="hidden" name="ctl00$CPH_PRINCIPAL$hfP" id="CPH_PRINCIPAL_hfP" value="kgDONRgkWtmWRNGJVdopY0JZBvfjybQSiMJXf7wJHI9nmbT15uAPxcdtWIkmZznKFOEEuCx%2f5wGGBh0EX%2fjE3w%3d%3d">
            <input type="hidden" name="ctl00$CPH_PRINCIPAL$hfR" id="CPH_PRINCIPAL_hfR" value="cyLJusIuLK3gJQs2NK3ekycrGRos8Gp7FqofQkq%2fN4uNDpJrTxrFXjwyE%2flQfoiTWwHV8gW7IAWjP0oCBPMk2Q%3d%3d">
            <input type="hidden" name="ctl00$CPH_PRINCIPAL$hfV" id="CPH_PRINCIPAL_hfV" value="IdoMRyGJAqiuehR%2fe%2bl8Fat%2fNSAzBcq91it%2bsZXvqhT5qiRkoc%2b1bkDKSilhmCwjJ%2bAloZ8HVsIwFVTuvFuz1g%3d%3d">
            <input type="hidden" name="ctl00$CPH_PRINCIPAL$hfRef" id="CPH_PRINCIPAL_hfRef" value="/referencia.aspx?p=kgDONRgkWtmWRNGJVdopY0JZBvfjybQSiMJXf7wJHI9nmbT15uAPxcdtWIkmZznKFOEEuCx%2f5wGGBh0EX%2fjE3w%3d%3d&amp;v=IdoMRyGJAqiuehR%2fe%2bl8Fat%2fNSAzBcq91it%2bsZXvqhT5qiRkoc%2b1bkDKSilhmCwjJ%2bAloZ8HVsIwFVTuvFuz1g%3d%3d&amp;r=cyLJusIuLK3gJQs2NK3ekycrGRos8Gp7FqofQkq%2fN4uNDpJrTxrFXjwyE%2flQfoiTWwHV8gW7IAWjP0oCBPMk2Q%3d%3d">
        </div>
    <div style="background-color: #e8e8e8; padding-bottom: 2%;">
        <div id="CPH_PRINCIPAL_uprMain" style="display: none;" role="status" aria-hidden="true">
	
                <div class="loading_container text-center">
                </div>
                <div class="loading_background">
                </div>
            
</div>
        <div id="CPH_PRINCIPAL_upMain">
                <div>
                    <div>
                        <br>
                        
                        
                        <div id="CPH_PRINCIPAL_pnlResultado">
	
                            <img src="./blank.gif" onload="javascript:showCodigoBarras()">
                            <div id="contenidoEstadoCuenta">
                                <img src="./vehiculoConAdeudo22.png" class="w-100" style="max-width: 612px" border="0" alt="Vehiculo con Adeudo">
                                <a href="https://icvnl.gob.mx/Certificado" target="_blank">
                                    
                                </a>
                                <div style="max-width: 612px" class="w-100 p-0 pb-3 text-right px-0 pt-2">
                                    
                                </div>
                                <br>
                                <div class="row m-0" style="margin: 0">
                                    <div class="w-100">
                                        <strong><legend>
                                            <span id="CPH_PRINCIPAL_lblPlaca">SSV195B</span></legend></strong>
                                    </div>
                                    <div class=" align-self-center" style="font-weight: bold">
                                        <strong><legend>
                                            <span id="CPH_PRINCIPAL_lblMarca">DODGE</span>
                                            <span id="CPH_PRINCIPAL_lblModelo">2010</span>,
                                            <span id="CPH_PRINCIPAL_lblLinea">CHALLENGER</span>,
                                            <span id="CPH_PRINCIPAL_lblTipo">CIL 6 2 PTAS 5 PAS COLOR:ROJO NIV:*****70274</span>
                                        </legend></strong>
                                    </div>
                                    <div class="w-100">
                                    </div>
                                    <div id="divTCIndefinidaMsg" class="">
                                    </div>
                                    <div class="w-100">
                                    </div>
                                    <div id="divStatus" style="display: none;">
                                        <div class="">
                                            <legend><strong>STATUS:
                                                <span id="CPH_PRINCIPAL_lblEstatus">0</span></strong></legend>
                                        </div>
                                        <div class="w-100">
                                        </div>
                                    </div>
                                    <div class="col col-md-auto p-0">
                                        <strong><legend style="font-size: 1.7rem;">Total</legend></strong>
                                    </div>
                                    <div class="col">
                                        <strong><legend>
                                            <span id="CPH_PRINCIPAL_lblTotal" style="font-size: 1.7rem; border-bottom: 2px solid black">$2,877.00</span></legend></strong>
                                    </div>
                                    <div class="w-100">
                                    </div>
                                    
                                    <div class="col-12 p-0">
                                        Su pago podrá ser recibido en cualquier sucursal de los siguientes bancos o instituciones
                                        autorizadas o mediante su banca electrónica de su respectivo banco.
                                    </div>
                                </div>
                                <br>
                                <div class="row m-0">
                                    <div class=" w-100">
                                        Línea de Referencia
                                    </div>
                                    <div class=" w-100">
                                        <strong>
                                            <span id="CPH_PRINCIPAL_lblLR">18000 00000 00000 43224 85430 82295</span></strong>
                                    </div>
                                    <div class="w-100">
                                    </div>
                                    <img id="codigoBarrasLR" src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAANwAAAAtCAYAAADFlAU9AAAAAXNSR0IArs4c6QAAAkhJREFUeF7t3NGSgjAMheH1/R96d1CrmdhSGZYMOt9euSgW/ub0hAS8/PhDAIEyApeykQyEAAI/BCcIECgkQHCFsA2FAMGJAQQKCRBcIWxDIUBwYgCBQgIEVwjbUAgQnBhAoJAAwRXCNhQCWwT3e8e17LO8bvu27cvbedvsM+272r5xjGXb2v/f+NnIK87N7FwzqzxXLdJHc9cbN85rntv4PfnY4rHEeZ2dz7uxk2MvxtDoPEfH9O7xrZ1T7/yHKwvB3dD0AnQWAHmx+I8FguDGi2xPHE18vcUhCpPggkueJdjP4LIER3APd5RSHu+GBEdwBBeuXY9OPwmO4AiO4K4E8mKQr596RZF3ixK9Ityo4KZoEkoys0LDaOLOcP10hmPgcByOw3E4Dhcq2dm1tQXuAvnE6ieH43AcjsNxOA63fhfK0dXEo673OByH43AcjsNxOA4XK75rjp7vdZ2V4HvleG2BZ7wpmnSKJFLKG5R8Y6+bl1+5ZCY5dlqKN1q4VClVKbs3Y/ea0L2nEDjca/YURcjhONyVwKgNIqW8BUiPQ28R4nCD5+xmz4jpw0kpZ7ebEdxKRfBTr8u2tBC0BbQFtAW0BbQFtAW0BbQFnj/14YnvgSuM+jlb0q5v/qyUUkoppZRSSimllFJKKaWU0s/k7XRDKaWUUkq5U0RbrjkJjuAegvMCAQR2EtjyQ7A7h7I7AggQnBhAoJAAwRXCNhQCBCcGECgkQHCFsA2FAMGJAQQKCRBcIWxDIUBwYgCBQgIEVwjbUAj8ARkxX3nP79JSAAAAAElFTkSuQmCC">
                                </div>
                                
                                <br>
                                <table>
                                    <tbody><tr>
                                        <td>
                                            <strong>Recuerda que puedes pagar solamente mencionando tu placa en:</strong>
                                        </td>
                                        <td>
                                            <img src="./banorteLogo.png" style="padding-left: 10px; padding-right: 10px" height="35px">
                                        </td>
                                        <td>
                                            <img src="./afirme_Logo.png" style="padding-left: 10px; padding-right: 10px" height="35px">
                                        </td>
                                        <!--<td>
                                            <img src="images/oxxoicv.png" style="padding-left: 10px; padding-right: 10px" height="35px">
                                        </td>-->
										<td>
                                            <img src="./logobanregio.png" style="padding-left: 10px; padding-right: 10px" height="35px">
                                        </td>
                                    </tr>
                                </tbody></table>
                                <br>
                                <div id="divBancos">
                                    <br>
                                    <table class="table table-sm  table-hover border-white" style="margin-top: 10px;">
                                        <tbody>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./banorteLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Empresa 3508 INSTITUTO DE CONTROL VEHICULAR DEL ESTADO NL
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./banamexLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Servicio 4552-01 ICV NUEVO LEON
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./bancomerLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">CONVENIO CIE: 1480138 INSTITUTO DE CONTROL VEHICULAR
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./scotiabankLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Servicio 4303 INSTITUTO DE CONTROL VEHICULAR DEL ESTADO DE NUEVO LEÓN
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./hsbcLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Servicio 583 INSTITUTO DE CONTROL VEHICULAR DEL ESTADO DE NUEVO LEÓN
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./santanderLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Servicio 9735 ICV NUEVO LEÓN
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./telecommLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Empresa 3508 INSTITUTO DE CONTROL VEHICULAR DEL ESTADO NL
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./azteca_Logo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">ICV NUEVO LEON
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./elektra_Logo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">ICV NUEVO LEON
                                                </td>
                                            </tr>
                                            <tr>
											</tr><tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./logobanregio.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Menciona tu <strong>Placa:
                                                        <span id="CPH_PRINCIPAL_lblPlacaO">SSV195B</span></strong>.
                                                </td>
                                            </tr>
                                            <tr>
                                                <td style="vertical-align: middle">
                                                    <img src="./7elevenLogo.png" height="30px">
                                                </td>
                                                <td style="vertical-align: middle">Facturador 3508 INSTITUTO DE CONTROL VEHICULAR DEL ESTADO NL **
                                                </td>
                                            </tr>
                                            <!--<tr>
                                                <td style="vertical-align: middle">
                                                    <img src="images/oxxoicv.png" height="30px" />
                                                </td>
                                                <td style="vertical-align: middle">Utiliza la Referencia de este Estado de Cuenta (Código de Barras) ó menciona tu
                                                    <strong>Placa:
                                                        <span id="CPH_PRINCIPAL_lblPlacaO2"></span></strong>.**
                                                </td>
                                            </tr>-->
                                            <tr>
                                                <td colspan="2">** Se aplicará cargo por servicio propio del establecimiento.
                                                </td>
                                            </tr>
                                        </tbody>
                                    </table>									
                                </div>
                                
                                        <table class="table table-striped table-sm table-borderless mb-0 ">
                                            <thead>
                                                <tr>
                                                    <th colspan="3">
                                                        <label style="font-size: 1.5rem; font-weight: bold; text-align: center; width: 100%">
                                                            CARGOS:</label>
                                                    </th>
                                                </tr>
                                                <tr>
                                                    <th colspan="3">
                                                        <legend><strong>Estatales</strong></legend>
                                                    </th>
                                                </tr>
                                                <tr>
                                                    <th scope="col">Concepto
                                                    </th>
                                                    <th scope="col">Año
                                                    </th>
                                                    <th scope="col">Importe
                                                    </th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                    
                                        <tr>
                                            <td style="width: 70%">
                                                <span id="CPH_PRINCIPAL_rptAdeudos_lblConcepto_0">REFRENDO PTE.AÑO</span>
                                            </td>
                                            <td style="width: 15%">
                                                <span id="CPH_PRINCIPAL_rptAdeudos_lblAnio_0">2024</span>
                                            </td>
                                            <td style="width: 15%">
                                                <span id="CPH_PRINCIPAL_rptAdeudos_lblConceptoTotal_0">$3,800.00</span>
                                            </td>
                                        </tr>
                                    
                                        <tr>
                                            <td style="width: 70%">
                                                <span id="CPH_PRINCIPAL_rptAdeudos_lblConcepto_1">SANCION REFRENDO PTE.AÑO</span>
                                            </td>
                                            <td style="width: 15%">
                                                <span id="CPH_PRINCIPAL_rptAdeudos_lblAnio_1">2024</span>
                                            </td>
                                            <td style="width: 15%">
                                                <span id="CPH_PRINCIPAL_rptAdeudos_lblConceptoTotal_1">$163.00</span>
                                            </td>
                                        </tr>
                                    
                                        </tbody> </table>
                                    
                                
                                <table class="table table-striped table-sm table-borderless  my-0">
                                    <tbody><tr style="background-color: #dddddd">
                                        <td style="width: 70%">
                                            <strong>SUBTOTAL CARGOS ESTATALES Y MUNICIPALES</strong>
                                        </td>
                                        <td style="width: 15%"></td>
                                        <td style="width: 15%">
                                            <strong>
                                                <span id="CPH_PRINCIPAL_lblTotalCargo">$3,963.00</span></strong>
                                        </td>
                                    </tr>
                                </tbody></table>
                                
                                        <table class="table table-striped table-sm table-borderless  my-0">
                                            <thead>
                                                <tr>
                                                    <th colspan="3">
                                                        <label style="font-size: 1.5rem; color: Black; font-weight: bold; text-align: center; width: 100%">
                                                            SUBSIDIOS Y OTROS DESCUENTOS:</label>
                                                    </th>
                                                </tr>
                                                <tr>
                                                    <th colspan="3">
                                                        <legend><strong>Estatales</strong></legend>
                                                    </th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                    
                                        <tr>
                                            <td style="width: 70%">
                                                <span id="CPH_PRINCIPAL_rptDescuentos_lblConcepto_0">SUBSIDIO ANTIGUEDAD 10 AÑOS</span>
                                            </td>
                                            <td style="width: 15%">
                                                <span id="CPH_PRINCIPAL_rptDescuentos_lblAnio_0">2024</span>
                                            </td>
                                            <td style="width: 15%">
                                                <span id="CPH_PRINCIPAL_rptDescuentos_lblConceptoTotal_0">-$1,086.00</span>
                                            </td>
                                        </tr>
                                    
                                        </tbody> </table>
                                        
                                    
                                <table class="table table-striped table-sm table-borderless  my-0">
                                    <tbody><tr style="background-color: #dddddd">
                                        <td>
                                            <strong>SUBTOTAL SUBSIDIOS Y OTROS DESCUENTOS ESTATALES</strong>
                                        </td>
                                        <td></td>
                                        <td>
                                            <strong>
                                                <span id="CPH_PRINCIPAL_lblTotalDescuentos">-$1,086.00</span></strong>
                                        </td>
                                    </tr>
                                </tbody></table>
                                <br>
                                <table class="table table-striped table-sm  mt-0">
                                    <tbody><tr style="background-color: #dddddd">
                                        <td style="width: 70%">
                                            <legend><strong>TOTAL A PAGAR</strong></legend>
                                        </td>
                                        <td style="width: 15%"></td>
                                        <td style="width: 15%">
                                            <legend><strong>
                                                <span id="CPH_PRINCIPAL_lblTotal2">$0.00</span>
                                            </strong></legend>
                                        </td>
                                    </tr>
                                </tbody></table>
                                <div class="row m-0 justify-content-end">
                                    <!--<div class="col col-md-auto"><strong><i>Vence el día <span id="CPH_PRINCIPAL_lblUltimoDiaMes">31/07/2024</span></i></strong></div>-->
                                    <div class="col col-md-auto">
                                        <strong><i>Vence el día 31/07/2024</i></strong>
                                    </div>
                                </div>
                                <div class="col-12">
                                    NOTA IMPORTANTE
                                </div>
                                <div class="col-12">
                                    - Este formato NO es un comprobante de pago.
                                </div>
                                <div class="col-12">
                                    - Los bancos autorizados solo reciben efectivo y cheques del mismo banco.
                                </div>
                                <!--<div class="col-12">
                                    - 4 días hábiles después de tu pago puedes descargar tu Tarjeta de Circulación Vehicular
                                    Complementaria en <a target="_blank" href="https://www.icvnl.gob.mx/TarjetadeCirculación">Tarjeta de Circulacion</a>
                                </div>-->
                                <div class="col-12">
                                    - Podrás solicitar tu factura electrónica (CFDI) después de 4 días hábiles en la
                                    página <a href="http://cfdi.nl.gob.mx/" target="_blank">http://cfdi.nl.gob.mx</a>
                                </div>
                                <br>
                                <br>
                                <div class="row">
                                    <div class="col col-md-auto">
                                        <strong><i>Si deseas pagar en Línea a través de la Tesorería Virtual da <a href="https://egobierno.nl.gob.mx/egob/PagoRefrendo.php" target="_blank">click aquí</a></i></strong>
                                    </div>
                                </div>
                                <br>
                                <br>
                                <!--
<p>**El programa Ponlo a tu nombre se realiza exclusivamente en las <a class="ligaRoja" href="http://www.nl.gob.mx/listado-ubicaciones/59332" target="_blank">Delegaciones del ICV</a> </p>
<p> **El programa Ponte al Corriente ya se encuentra aplicado en este Estado de Cuenta</p> 
<img src="images/promociones.png" border="0" alt="Programas Actuales" height="550px" />-->
                            </div>
                            <div class="row m-0">
                                <div class="col-sm-12 col-md-8">
                                    <div id="msgSuccessMail" class="col alert alert-success" style="display: none;" role="alert">
                                        Se ha enviado un correo con el estado de cuenta!
                                    </div>
                                    <div id="msgErrorMail" class="col alert alert-danger" style="display: none;" role="alert">
                                    </div>
                                </div>
                                <div class="w-100">
                                </div>
                                <div class="col-sm-12 col-md-4 p-0">
                                    <div class="input-group margin-bottom-sm">
                                        <span class="input-group-addon"><i class="fa fa-envelope-o fa-fw"></i></span>
                                        <input type="text" id="txtCorreo" placeholder="Correo" class="form-control" maxlength="50" onkeypress="return disableEnterKey(event)">
                                    </div>
                                </div>
                                <div class="w-100">
                                </div>
                                <div class="col-sm-12 col-md-4 p-0">
                                    <a id="btnEnviarCorreo" class="btn btn-sm btn-block btn-danger mb-1" href="https://www.icvnl.gob.mx:1034/edocuenta24/Consultaestadocuenta.aspx#" onclick="javascript:EnviarPorCorreo();">
                                        <i class="fa fa-envelope"></i>Enviar al correo </a>
                                </div>
                                <div class="col-sm-12 col-md-4 p-0">
                                    <a class="btn btn-sm btn-block btn-danger mb-1" href="https://www.icvnl.gob.mx:1034/edocuenta24/Consultaestadocuenta.aspx#" onclick="javascript:Imprimir(document.getElementById(&#39;contenidoEstadoCuenta&#39;));">
                                        <i class="fa fa-print"></i>Imprimir </a>
                                </div>
                                <div class="col-sm-12 col-md-4 p-0">
                                    <a id="btnRegresar" href="https://www.icvnl.gob.mx:1034/edocuenta24/ConsultaEstadoCuenta.aspx" class="btn btn-sm btn-block btn-danger mb-1">
                                        <i class="fa fa-arrow-left"></i>Regresar </a>
                                </div>
                            </div>
                            <br>
                        
</div>
                        
                        
                    </div>
                    <label class=" w-75 text-right" id="lblVersion">
                        Versión 1.6</label>
                </div>
                <script src="./iframeResizer.contentWindow.min.js.descarga" type="text/javascript"></script>
            </div>
    </div>

        

<script type="text/javascript">
//<![CDATA[
Sys.Application.add_init(function() {
    $create(Sys.UI._UpdateProgress, {"associatedUpdatePanelId":"CPH_PRINCIPAL_upMain","displayAfter":250,"dynamicLayout":true}, null, null, $get("CPH_PRINCIPAL_uprMain"));
});
//]]>
</script>
<span style="display: none !important;"><input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="/wEPDwUKLTU0MjIzMjU1OQ9kFgJmD2QWAgIDD2QWAgIDD2QWAgIFD2QWAmYPZBYEAgEPDxYCHgdWaXNpYmxlaGQWBgIBDw8WAh4EVGV4dAUHU1NWMTk1QmRkAgMPEA8WAh4HQ2hlY2tlZGhkZGRkAgsPZBYCAgEPDxYCHwFlZGQCBQ8PFgIfAGdkFhwCBQ8PFgIfAQUHU1NWMTk1QmRkAgcPDxYCHwEFBURPREdFZGQCCQ8PFgIfAQUEMjAxMGRkAgsPDxYCHwEFCkNIQUxMRU5HRVJkZAINDw8WAh8BBSxDSUwgNiAyIFBUQVMgNSBQQVMgQ09MT1I6Uk9KTyBOSVY6KioqKio3MDI3NGRkAg8PDxYCHwEFATBkZAIRDw8WAh8BBQkkMiw4NzcuMDBkZAIVDw8WAh8BBSMxODAwMCAwMDAwMCAwMDAwMCA0MzIyNCA4NTQzMCA4MjI5NWRkAh0PFgIeC18hSXRlbUNvdW50AgIWBAIBD2QWBgIBDw8WAh8BBRFSRUZSRU5ETyBQVEUuQcORT2RkAgMPDxYCHwEFBDIwMjRkZAIFDw8WAh8BBQkkMyw4MDAuMDBkZAICD2QWBgIBDw8WAh8BBRlTQU5DSU9OIFJFRlJFTkRPIFBURS5Bw5FPZGQCAw8PFgIfAQUEMjAyNGRkAgUPDxYCHwEFByQxNjMuMDBkZAIhDw8WAh8BBQkkMyw5NjMuMDBkZAIjDxYCHwMCARYCAgEPZBYGAgEPDxYCHwEFHFNVQlNJRElPIEFOVElHVUVEQUQgMTAgQcORT1NkZAIDDw8WAh8BBQQyMDI0ZGQCBQ8PFgIfAQUKLSQxLDA4Ni4wMGRkAiUPDxYCHwEFCi0kMSwwODYuMDBkZAInDw8WAh8BBQkkMiw4NzcuMDBkZAIpDw8WAh8BBQozMS8wNy8yMDI0ZGRkUD9AA4cqClyqLtzlYN9HCdZzUvwyf+7cP2zQX3iSO0Q="></span><span style="display: none !important;"><input type="hidden" name="__VIEWSTATEGENERATOR" id="__VIEWSTATEGENERATOR" value="B5B1F861"></span><span style="display: none !important;"><input type="hidden" name="__EVENTVALIDATION" id="__EVENTVALIDATION" value="/wEdAAVi7A4Vp5+mUsAwUxo53AIiOUggAcHVN526A9NQRe3IvpuaN7uaamzIh3eq1WWA+8gcZlkl/cGHO4wZ6cm1xD2JSc8KlVLpk8w1zX34EmMPwf+xF67WAMQ/OXHXZ8Z4kSJ10hMkr8aA/HKtPBdS2Ux4"></span></form>
    </div>







<div style="clear: both; display: block; height: 0px;"></div></body></html>
