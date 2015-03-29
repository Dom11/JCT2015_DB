# JCT2015_DB
Database for JCT2015 application


SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='TRADITIONAL,ALLOW_INVALID_DATES';

CREATE SCHEMA IF NOT EXISTS `jct_profiles` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci ;
USE `jct_profiles` ;

-- -----------------------------------------------------
-- Table `jct_profiles`.`environment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`environment` (
  `environment_id` INT NOT NULL AUTO_INCREMENT,
  `environmentName` VARCHAR(5) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`environment_id`),
  UNIQUE INDEX `environmentName_UNIQUE` (`environmentName` ASC))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `jct_profiles`.`host`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`host` (
  `host_id` INT NOT NULL AUTO_INCREMENT,
  `hostName` VARCHAR(20) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`host_id`),
  UNIQUE INDEX `hostName_UNIQUE` (`hostName` ASC))
ENGINE = InnoDB
COMMENT = '	';


-- -----------------------------------------------------
-- Table `jct_profiles`.`jbar`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`jbar` (
  `jbar_id` INT NOT NULL AUTO_INCREMENT,
  `jbarName` VARCHAR(5) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`jbar_id`),
  UNIQUE INDEX `jbarName_UNIQUE` (`jbarName` ASC))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `jct_profiles`.`jira`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`jira` (
  `jira_id` INT NOT NULL AUTO_INCREMENT,
  `jiraProjectKey` VARCHAR(45) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`jira_id`),
  UNIQUE INDEX `jiraProjectKey_UNIQUE` (`jiraProjectKey` ASC))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `jct_profiles`.`prefix`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`prefix` (
  `prefix_id` INT NOT NULL AUTO_INCREMENT,
  `prefixName` VARCHAR(5) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`prefix_id`),
  UNIQUE INDEX `prefixName_UNIQUE` (`prefixName` ASC))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `jct_profiles`.`domain`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`domain` (
  `domain_id` INT NOT NULL AUTO_INCREMENT,
  `domainName` VARCHAR(45) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`domain_id`),
  UNIQUE INDEX `domainNaime_UNIQUE` (`domainName` ASC))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `jct_profiles`.`jvmArgument`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`jvmArgument` (
  `jvm_id` INT NOT NULL AUTO_INCREMENT,
  `jvmArgument` VARCHAR(255) NOT NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`jvm_id`),
  UNIQUE INDEX `jvmArgument_UNIQUE` (`jvmArgument` ASC))
ENGINE = InnoDB;


-- -----------------------------------------------------
-- Table `jct_profiles`.`profile`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`profile` (
  `profile_id` INT NOT NULL AUTO_INCREMENT,
  `profileDescription` VARCHAR(255) NULL,
  `profileComponent` VARCHAR(255) NULL DEFAULT '-',
  `profileDnsName` VARCHAR(255) NULL,
  `environment_id` INT NOT NULL,
  `host_id` INT NOT NULL,
  `jbar_id` INT NOT NULL,
  `jira_id` INT NOT NULL,
  `prefix_id` INT NOT NULL,
  `domain_id` INT NOT NULL,
  `jvm_id` INT NULL DEFAULT 0,
  `profileStatus` TINYINT(1) NULL DEFAULT false,
  `createdBy` VARCHAR(100) NULL,
  `rpmGenerationDate` DATETIME NULL,
  `packageSentDate` DATETIME NULL,
  `version` INT NULL DEFAULT 0,
  PRIMARY KEY (`profile_id`),
  INDEX `fk_profile_environment_idx` (`environment_id` ASC),
  INDEX `fk_profile_host1_idx` (`host_id` ASC),
  INDEX `fk_profile_jbar1_idx` (`jbar_id` ASC),
  INDEX `fk_profile_jira1_idx` (`jira_id` ASC),
  INDEX `fk_profile_prefix1_idx` (`prefix_id` ASC),
  UNIQUE INDEX `name_id` (`prefix_id` ASC, `jbar_id` ASC, `environment_id` ASC),
  INDEX `fk_profile_domain1_idx` (`domain_id` ASC),
  INDEX `fk_profile_jvmArgument1_idx` (`jvm_id` ASC),
  CONSTRAINT `fk_profile_environment`
    FOREIGN KEY (`environment_id`)
    REFERENCES `jct_profiles`.`environment` (`environment_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_profile_host1`
    FOREIGN KEY (`host_id`)
    REFERENCES `jct_profiles`.`host` (`host_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_profile_jbar1`
    FOREIGN KEY (`jbar_id`)
    REFERENCES `jct_profiles`.`jbar` (`jbar_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_profile_jira1`
    FOREIGN KEY (`jira_id`)
    REFERENCES `jct_profiles`.`jira` (`jira_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_profile_prefix1`
    FOREIGN KEY (`prefix_id`)
    REFERENCES `jct_profiles`.`prefix` (`prefix_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_profile_domain1`
    FOREIGN KEY (`domain_id`)
    REFERENCES `jct_profiles`.`domain` (`domain_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT `fk_profile_jvmArgument1`
    FOREIGN KEY (`jvm_id`)
    REFERENCES `jct_profiles`.`jvmArgument` (`jvm_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
COMMENT = '			';

USE `jct_profiles` ;

-- -----------------------------------------------------
-- Placeholder table for view `jct_profiles`.`profileView`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `jct_profiles`.`profileView` (`profile_id` INT, `profileName` INT, `profileDescription` INT, `profileComponent` INT, `profileDnsName` INT, `environmentName` INT, `hostName` INT, `jbarName` INT, `jiraProjectKey` INT, `prefixName` INT, `domainName` INT, `profileStatus` INT, `jvmArgument` INT, `createdBy` INT, `rpmGenerationDate` INT, `packageSentDate` INT);

-- -----------------------------------------------------
-- View `jct_profiles`.`profileView`
-- -----------------------------------------------------
DROP TABLE IF EXISTS `jct_profiles`.`profileView`;
USE `jct_profiles`;
CREATE  OR REPLACE VIEW `profileView` AS
SELECT 
    profile_id,
	Concat(prefixName, '_', jbarName, '_', environmentName) AS profileName,
    profileDescription,
    profileComponent,
    profileDnsName,
    environmentName,
    hostName,
    jbarName,
    jiraProjectKey,
    prefixName,
	domainName,
	profileStatus,
	jvmArgument,
	createdBy,
	rpmGenerationDate,
	packageSentDate
FROM
    profile, environment, host, jbar, jira, prefix, domain, jvmargument
WHERE
    (profile.environment_id = environment.environment_id)
        AND (profile.host_id = host.host_id)
        AND (profile.jbar_id = jbar.jbar_id)
        AND (profile.jira_id = jira.jira_id)
        AND (profile.prefix_id = prefix.prefix_id)
		AND (profile.domain_id = domain.domain_id)
		AND (profile.jvm_id = jvmargument.jvm_id);

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;

