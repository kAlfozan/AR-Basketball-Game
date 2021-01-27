# AR-Basketball-Game


package com.netzima.midcaixabank.midcaixabank.sensitivedata.customimpl;

import java.util.concurrent.ThreadLocalRandom;
import com.netzima.midcaixabank.midcaixabank.sensitivedata.impl.*;

public class SensitiveDataCustom_SDCaixaGeolocation extends SensitiveData_SDCaixaGeolocation {

	private int exceptionCounter = 0;

	protected boolean customDissociated() {

		try {
			if ((this.fieldValue_GEOLOCATIONX == null || this.fieldValue_GEOLOCATIONX.trim().length() == 0)
					|| (this.fieldValue_GEOLOCATIONY == null || this.fieldValue_GEOLOCATIONY.trim().length() == 0)) {
				return true;
			}
			return customDissociatedInternal();
		} catch (RuntimeException ex) {
			exceptionCounter++;
			this.setNewFieldValue_GEOLOCATIONX("" + exceptionCounter);
			this.setNewFieldValue_GEOLOCATIONY("" + exceptionCounter);
			if (exceptionCounter == 1) {
				ex.printStackTrace();
			} else if (exceptionCounter < 100) {
				String tableName = this.getTableDissociator().getEntity();
				this.getLog().error("Geolocation dissociator for table " + tableName + " produced an exception: "
						+ ex.getMessage() + this.newFieldValue_GEOLOCATIONX + ", " + this.newFieldValue_GEOLOCATIONY);
			}
			return true;
		}

	}

	private boolean customDissociatedInternal() {
		this.setNewFieldValue_GEOLOCATIONX(customDissociatedInternal1(this.fieldValue_GEOLOCATIONX.trim()));
		this.setNewFieldValue_GEOLOCATIONY(customDissociatedInternal1(this.fieldValue_GEOLOCATIONY.trim()));
		return true;
	}

	private String customDissociatedInternal1(String coord) {
		String result = null;
		if (!coord.contains(".") || !coord.contains(",")) {
			return coord.substring(0, coord.length() - 1) + numericDance();
		}

		int indexOfSeparator = coord.indexOf(".");

		if (indexOfSeparator == -1) {
			indexOfSeparator = coord.indexOf(",");
		}

		result = coord.substring(0, indexOfSeparator + 1);

		for (int i = indexOfSeparator + 1; i < coord.length(); i++) {
			result += numericDance();
		}

		return result;

	}

	public String numericDance() {
		return Integer.toString(numeroAleatorioEnRango(0, 10));
	}

	public int numeroAleatorioEnRango(int minimo, int maximo) {
		return ThreadLocalRandom.current().nextInt(minimo, maximo);
	}
}
