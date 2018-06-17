package pe.com.test.seleniumwd;

import org.testng.Assert;
import org.testng.ITestContext;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

import pe.com.test.seleniumwd.fuenteDatos.Excel2;
import pe.com.test.seleniumwd.fuenteDatos.MySql;
import pe.com.test.seleniumwd.page.ExamenPage;
import pe.com.test.seleniumwd.page.IniciarSesionPageExamen;
import pe.com.test.seleniumwd.util.Utilitario;;

public class ExamenWebDriverTest {

	private String urlInicial = "http://localhost:8082/TPidoWeb/";
	private ExamenPage examenPage;
	private IniciarSesionPageExamen iniciarSesionPageExamen;
	
	private String rutaCarpetaError = "C:\\CapturasPantallas\\Examen";

	@BeforeTest
	@Parameters({ "navegador", "remoto" })
	public void inicioClase(String navegador, int remoto) throws Exception {
		this.iniciarSesionPageExamen = new IniciarSesionPageExamen(navegador, this.urlInicial, remoto == 1);
		this.examenPage = new ExamenPage(this.iniciarSesionPageExamen.getWebDriver());
	}

	@DataProvider(name = "datosEntrada")
	public static Object[][] datosPoblados(ITestContext context) {
		
		Object[][] datos = null;//de aca comienzo
		//TODO
		String fuenteDatos = context.getCurrentXmlTest().getParameter("fuenteDatos");
		
		System.out.println("Fuente de datos : " + fuenteDatos);
		
		switch (fuenteDatos){
		//case "BD":
			//datos = MySql.leerProductoMysql();
			//break;
		case "Excel":
			String rutaRegistrar = context.getCurrentXmlTest().getParameter("rutaRegistrar");
			datos = Excel2.leerExcel(rutaRegistrar);
		}
		return datos;
	}
	
	@DataProvider(name = "datosEntrada2")
	public static Object[][] datosPoblados2(ITestContext context) {
		
		Object[][] datos = null;//de aca comienzo
		//TODO
		String fuenteDatos = context.getCurrentXmlTest().getParameter("fuenteDatos");
		
		System.out.println("Fuente de datos : " + fuenteDatos);
		
		switch (fuenteDatos){
		//case "BD":
			//datos = MySql.leerProductoMysql();
			//break;
		case "Excel":
			String rutaPedido = context.getCurrentXmlTest().getParameter("rutaPedido");
			datos = Excel2.leerExcel2(rutaPedido);
		}
		return datos;
	}
	
	@Test(dataProvider = "datosEntrada")
	public void insertarUsuario(String nombre, String apellido, String correo, String clave, String valorEsperado) throws Exception {
		try {
			//iniciarSesionPageExamen.iniciarSesion(usuario, clave);
			String valorObtenido = examenPage.insertar(nombre, apellido,correo,clave);
			Assert.assertEquals(valorObtenido, valorEsperado);
		} catch(AssertionError e) {
			Utilitario.caputarPantallarError(rutaCarpetaError,e.getMessage(),examenPage.getWebDriver());
			//se indica que manualmente tiene que fallar por que ya se esta controlando
			Assert.fail(e.getMessage()); //o sea ahora si lanzalo como error
		} catch (Exception e) {
			e.printStackTrace();
			Assert.fail();
		}
	}
	
	@Test(dataProvider = "datosEntrada2",dependsOnMethods="insertarUsuario")
	public void insertarPedido(String usuario,String clave, String producto, String cantidad,  String valorEsperado) throws Exception {
		try {
			iniciarSesionPageExamen.iniciarSesionExamen(usuario, clave);
			String valorObtenido = examenPage.insertar2(producto.trim(),cantidad.trim());
			Assert.assertEquals(valorObtenido, valorEsperado);
		} catch(AssertionError e) {
			Utilitario.caputarPantallarError(rutaCarpetaError,e.getMessage(),examenPage.getWebDriver());
			//se indica que manualmente tiene que fallar por que ya se esta controlando
			Assert.fail(e.getMessage()); //o sea ahora si lanzalo como error
		} catch (Exception e) {
			e.printStackTrace();
			Assert.fail();
		}
	}
	
	@AfterTest
	public void tearDown() throws Exception {
		examenPage.cerrarPagina();
	}

}
